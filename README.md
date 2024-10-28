# rock-paper-scissors-game
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() 
{
    int userChoice, computerChoice;
    const char *choices[] = {"Rock", "Paper", "Scissors"};

    srand(time(0));

    printf("Welcome to Rock-Paper-Scissors!\n");
    printf("Enter your choice:\n");
    printf("0: Rock\n1: Paper\n2: Scissors\n");


    printf("Your choice: ");
    scanf("%d", &userChoice);


    if (userChoice < 0 || userChoice > 2) {
        printf("Invalid choice! Please choose 0, 1, or 2.\n");
        return 1;
    }


    computerChoice = rand() % 3;
    printf("Computer chose: %s\n", choices[computerChoice]);


    if (userChoice == computerChoice) {
        printf("It's a tie!\n");
    } else if ((userChoice == 0 && computerChoice == 2) ||
               (userChoice == 1 && computerChoice == 0) ||
               (userChoice == 2 && computerChoice == 1)) {
        printf("You win!\n");
    } else {
        printf("You lose!\n");
    }

    return 0;
}
