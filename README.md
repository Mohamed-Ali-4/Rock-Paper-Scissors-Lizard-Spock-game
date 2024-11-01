# rock-paper-scissors-game with login credentials

#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <time.h>

// Predefined login credentials
#define USERNAME "admin"
#define PASSWORD "1234"

// Function declarations
int login();
void playRockPaperScissors();

int main() {
    // Run the login function
    if (login()) {
        printf("Login successful! Starting the Rock-Paper-Scissors game...\n");
        playRockPaperScissors();
    } else {
        printf("Too many failed attempts. Access denied.\n");
    }

    return 0;
}

// Login function
int login() {
    char inputUsername[20];
    char inputPassword[20];
    int loginAttempt = 0;
    int maxAttempts = 3;

    while (loginAttempt < maxAttempts) {
        printf("Enter Username: ");
        scanf("%s", inputUsername);

        printf("Enter Password: ");
        scanf("%s", inputPassword);

        // Check credentials
        if (strcmp(inputUsername, USERNAME) == 0 && strcmp(inputPassword, PASSWORD) == 0) {
            return 1; // Login successful
        } else {
            printf("Invalid credentials. Try again.\n");
            loginAttempt++;
        }
    }

    return 0; // Login failed after max attempts
}

// Rock-Paper-Scissors game function
void playRockPaperScissors() {
    char *choices[] = {"Rock", "Paper", "Scissors"};
    int userChoice, computerChoice;
    char playAgain;

    srand(time(0)); // Seed the random number generator

    do {
        printf("\nChoose an option:\n");
        printf("1. Rock\n");
        printf("2. Paper\n");
        printf("3. Scissors\n");
        printf("Enter your choice (1-3): ");
        scanf("%d", &userChoice);

        // Check for valid user input
        if (userChoice < 1 || userChoice > 3) {
            printf("Invalid choice! Please enter a number between 1 and 3.\n");
            continue;
        }

        userChoice--; // Adjust user choice for 0-based index
        computerChoice = rand() % 3; // Generate computer's choice (0, 1, or 2)

        printf("You chose %s\n", choices[userChoice]);
        printf("Computer chose %s\n", choices[computerChoice]);

        // Determine the result
        if (userChoice == computerChoice) {
            printf("It's a tie!\n");
        } else if ((userChoice == 0 && computerChoice == 2) ||
                   (userChoice == 1 && computerChoice == 0) ||
                   (userChoice == 2 && computerChoice == 1)) {
            printf("You win!\n");
        } else {
            printf("Computer wins!\n");
        }

        // Prompt user to play again
        printf("Do you want to play again? (y/n): ");
        scanf(" %c", &playAgain);

    } while (playAgain == 'y' || playAgain == 'Y');

    printf("Thanks for playing!\n");
}
