#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define MAX_USERS 100
#define USER_FILE "users.txt"

// Function prototypes
void register_user();
int login_user(char* username);
const char* determine_winner(const char* player, const char* computer);
void get_computer_choice(char* computer, const char* history[], int round, const char* difficulty);
int is_valid_choice(const char* choice);
void play_game(const char* username, int* total_games, int* user_wins, int* computer_wins, int* ties);

// Function to register a new user
void register_user() {
    char username[50], password[50];

    printf("Enter a username: ");
    scanf("%s", username);
    printf("Enter a password: ");
    scanf("%s", password);

    // Save the user to the file
    FILE* file = fopen(USER_FILE, "a");
    if (file == NULL) {
        printf("Error opening file for registration.\n");
        return;
    }

    fprintf(file, "%s %s\n", username, password);
    fclose(file);

    printf("Registration successful!\n");
}

// Function to authenticate a user
int login_user(char* username) {
    char input_username[50], input_password[50], file_username[50], file_password[50];

    printf("Enter your username: ");
    scanf("%s", input_username);
    printf("Enter your password: ");
    scanf("%s", input_password);

    // Read users from the file
    FILE* file = fopen(USER_FILE, "r");
    if (file == NULL) {
        printf("Error opening file for login.\n");
        return 0;
    }

    while (fscanf(file, "%s %s", file_username, file_password) != EOF) {
        if (strcmp(input_username, file_username) == 0 && strcmp(input_password, file_password) == 0) {
            strcpy(username, input_username);
            fclose(file);
            printf("Login successful!\n");
            return 1;
        }
    }

    fclose(file);
    printf("Invalid username or password.\n");
    return 0;
}

// Function to determine the winner
const char* determine_winner(const char* player, const char* computer) {
    if (strcmp(player, computer) == 0) return "It's a tie!";
    if ((strcmp(player, "rock") == 0 && (strcmp(computer, "scissors") == 0 || strcmp(computer, "lizard") == 0)) ||
        (strcmp(player, "paper") == 0 && (strcmp(computer, "rock") == 0 || strcmp(computer, "spock") == 0)) ||
        (strcmp(player, "scissors") == 0 && (strcmp(computer, "paper") == 0 || strcmp(computer, "lizard") == 0)) ||
        (strcmp(player, "lizard") == 0 && (strcmp(computer, "paper") == 0 || strcmp(computer, "spock") == 0)) ||
        (strcmp(player, "spock") == 0 && (strcmp(computer, "rock") == 0 || strcmp(computer, "scissors") == 0))) {
        return "You win!";
    }
    return "Computer wins!";
}

// Function to get the computer's choice
void get_computer_choice(char* computer, const char* history[], int round, const char* difficulty) {
    const char* moves[] = {"rock", "paper", "scissors", "lizard", "spock"};
    if (strcmp(difficulty, "easy") == 0) {
        strcpy(computer, moves[rand() % 5]);
    } else if (strcmp(difficulty, "medium") == 0) {
        const char* player_last = history[round - 1];
        if (strcmp(player_last, "rock") == 0) strcpy(computer, "paper");
        else if (strcmp(player_last, "paper") == 0) strcpy(computer, "scissors");
        else if (strcmp(player_last, "scissors") == 0) strcpy(computer, "rock");
        else if (strcmp(player_last, "lizard") == 0) strcpy(computer, "rock");
        else if (strcmp(player_last, "spock") == 0) strcpy(computer, "lizard");
        else strcpy(computer, moves[rand() % 5]);
    } else if (strcmp(difficulty, "hard") == 0) {
        int counts[5] = {0};
        for (int i = 0; i < round; i++) {
            if (strcmp(history[i], "rock") == 0) counts[0]++;
            else if (strcmp(history[i], "paper") == 0) counts[1]++;
            else if (strcmp(history[i], "scissors") == 0) counts[2]++;
            else if (strcmp(history[i], "lizard") == 0) counts[3]++;
            else if (strcmp(history[i], "spock") == 0) counts[4]++;
        }
        int most_common = 0;
        for (int i = 1; i < 5; i++) {
            if (counts[i] > counts[most_common]) most_common = i;
        }
        if (most_common == 0) strcpy(computer, "paper");
        else if (most_common == 1) strcpy(computer, "scissors");
        else if (most_common == 2) strcpy(computer, "rock");
        else if (most_common == 3) strcpy(computer, "rock");
        else if (most_common == 4) strcpy(computer, "lizard");
    }
}

// Function to validate player input
int is_valid_choice(const char* choice) {
    return strcmp(choice, "rock") == 0 || strcmp(choice, "paper") == 0 ||
           strcmp(choice, "scissors") == 0 || strcmp(choice, "lizard") == 0 ||
           strcmp(choice, "spock") == 0;
}

// Function to play the game
void play_game(const char* username, int* total_games, int* user_wins, int* computer_wins, int* ties) {
    char player[10], computer[10], difficulty[10];
    const char* history[100];
    int player_score = 0, computer_score = 0, rounds;

    printf("Choose difficulty (easy, medium, hard): ");
    scanf("%s", difficulty);

    printf("How many rounds do you want to play? ");
    scanf("%d", &rounds);

    for (int i = 0; i < rounds; i++) {
        printf("\nRound %d:\n", i + 1);

        do {
            printf("Enter your choice (rock, paper, scissors, lizard, spock): ");
            scanf("%s", player);
            if (!is_valid_choice(player)) {
                printf("Invalid choice! Please choose one of rock, paper, scissors, lizard, or spock.\n");
            }
        } while (!is_valid_choice(player));

        history[i] = strdup(player);
        get_computer_choice(computer, history, i, difficulty);

        printf("Computer chose: %s\n", computer);
        const char* result = determine_winner(player, computer);
        printf("%s\n", result);

        if (strcmp(result, "You win!") == 0) {
            player_score++;
            (*user_wins)++;
        } else if (strcmp(result, "Computer wins!") == 0) {
            computer_score++;
            (*computer_wins)++;
        } else {
            (*ties)++;
        }

        free((void*)history[i]);
    }

    (*total_games)++;

    printf("\nFinal Score:\nYou: %d\nComputer: %d\n", player_score, computer_score);
    if (player_score > computer_score) printf("You are the overall winner!\n");
    else if (player_score < computer_score) printf("Computer is the overall winner!\n");
    else printf("It's an overall tie!\n");

    char play_again;
    printf("Do you want to play again? (y/n): ");
    scanf(" %c", &play_again);
    if (play_again == 'y' || play_again == 'Y') {
        play_game(username, total_games, user_wins, computer_wins, ties);
    } else {
        printf("\nScoreboard:\n");
        printf("Total Games Played: %d\n", *total_games);
        printf("Your Wins: %d\n", *user_wins);
        printf("Computer Wins: %d\n", *computer_wins);
        printf("Ties: %d\n", *ties);
        printf("Thanks for playing, %s!\n", username);
    }
}

int main() {
    char username[50];
    int total_games = 0, user_wins = 0, computer_wins = 0, ties = 0;

    printf("Welcome to Rock, Paper, Scissors, Lizard, Spock with Authentication!\n");
    printf("1. Register\n2. Login\nChoose an option: ");

    int option;
    scanf("%d", &option);

    if (option == 1) {
        register_user();
        return 0;
    } else if (option == 2) {
        if (!login_user(username)) return 0;
    } else {
        printf("Invalid option. Exiting.\n");
        return 0;
    }

    play_game(username, &total_games, &user_wins, &computer_wins, &ties);
    return 0;
}
