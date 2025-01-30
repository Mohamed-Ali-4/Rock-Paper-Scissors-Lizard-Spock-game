# Rock, Paper, Scissors, Lizard, Spock with Authentication

## Description
This is a command-line game that allows users to play an extended version of Rock, Paper, Scissors (including Lizard and Spock). The program includes user authentication with registration and login functionalities to save and authenticate users.

## Features
- User Registration: New users can create an account.
- User Login: Existing users can log in.
- Difficulty Levels: Choose from easy, medium, and hard modes.
- Game Mechanics:
  - Rock (1), Paper (2), Scissors (3), Lizard (4), Spock (5)
  - AI opponent with difficulty scaling based on previous choices.
- Scoring System: The winner is determined based on multiple rounds.
- Replay Option: Users can choose to play again after finishing a game.

## How to Use
1. Compile the program using GCC:
   ```sh
   gcc -o game game.c
   ```
2. Run the program:
   ```sh
   ./game
   ```
3. Choose an option:
   - Register a new user.
   - Login with an existing user.
4. Select the difficulty level: easy, medium, or hard.
5. Choose the number of rounds.
6. Play the game by selecting numbers corresponding to Rock, Paper, Scissors, Lizard, or Spock.
7. View the final score and decide whether to play again.

## Game Rules
- Rock (1) crushes Scissors (3) and crushes Lizard (4)
- Paper (2) covers Rock (1) and disproves Spock (5)
- Scissors (3) cuts Paper (2) and decapitates Lizard (4)
- Lizard (4) eats Paper (2) and poisons Spock (5)
- Spock (5) smashes Scissors (3) and vaporizes Rock (1)

## File Structure
- `game.c`: Main source code file.
- `users.txt`: Stores registered usernames and passwords.

## Notes
- User credentials are stored in `users.txt` in plain text. Consider adding encryption for enhanced security.
- The AI difficulty system predicts user choices based on previous rounds in medium and hard modes.

## Future Improvements
- Implement user data encryption for security.
- Add a graphical user interface (GUI).
- Store user scores and maintain a leaderboard.

## License
This project is released under the MIT License.

