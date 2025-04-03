# Code-Quest
Current version 1.0
Basic functionality, multple variables and ways to go, but there is a lot more story to add to the game


<img width="728" alt="Screenshot 2025-03-28 at 12 03 44 PM" src="https://github.com/user-attachments/assets/7e0a8d49-2f0e-4f4f-9170-1dc3be3106fd" />


![pixil-gif-drawing](https://github.com/user-attachments/assets/4cce6ab2-45c5-40df-bad6-568fe0b61ace)


# Script For the Game (does contain spoilers)


https://docs.google.com/document/d/1TNB_FNX1GAQDz3eZ701NDPBMGUbn54_kbPIoXaTQta8/edit?tab=t.0


source code:
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class MyProgram {
    public static void main(String[] args) {
        while (true) {  // Main game loop for restarting
            startGame();
        }
    }

    public static void startGame() {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Welcome To Code Quest");
        System.out.println("Want to play? y/n");

        int count = 0;
        int items = 0;
        boolean hasTorch = false;
        boolean lastMessageWasBrainDead = false;
        String play;

        // Refusal messages
        String[] messages = {
            "Do you want to play?", "Do you want to play?", "Do you want to play?",
            "Just press y. It's really easy.", "What do you want? (press y if you want to play)",
            "You're starting to annoy me. Just press y.", "...", "...",
            "Are you brain dead or something?", "...", 
            "Fine, you win. Do you want a prize?", "If I give you something, will you play?",
            "Okay, this is getting ridiculous...", "Press y or else",
            "You should have pressed y. Terminating..."
        };

        // Player consent loop
        do {
            play = scanner.nextLine().trim();
            if (play.equalsIgnoreCase("n")) {
                System.out.println(messages[count % messages.length]);
                lastMessageWasBrainDead = (count % messages.length == 8);
                count++;
                if (count >= 15) {
                    System.exit(0);
                }
            } else if (play.equalsIgnoreCase("y")) {
                if (lastMessageWasBrainDead) {
                    System.out.println("Yeah, that's what I thought.");
                }
                break;
            }
        } while (true);

        // Player starts with a torch if they refused long enough
        if (count >= 11) {
            hasTorch = true;
            items++;
            System.out.println("You find yourself clutching a torch.");
        }

        // Map Setup (2D Coordinates -> Description)
        Map<String, String> worldMap = new HashMap<>();
        worldMap.put("0,0", "You are at the cave entrance. There's a note on the ground.");
        worldMap.put("0,1", "You step into the dark cave. It's too dark to see anything.");
        worldMap.put("0,-1", "You step back outside into the sunlight.");
        worldMap.put("1,0", "To the east, you see a pile of rubble blocking a passage.");
        worldMap.put("-1,0", "To the west, there's an old wooden bridge, looking fragile.");

        int playerX = 0, playerY = 0;  // Player position

        // Game Loop
        while (true) {
            System.out.println("\n" + worldMap.getOrDefault(playerX + "," + playerY, "You don't know where you are."));

            System.out.print("\nWhat do you want to do? ");
            String choice = scanner.nextLine().trim().toLowerCase();

            if (choice.contains("read note") || choice.contains("take note") || choice.contains("pick up note") || 
                choice.contains("look at note") || choice.contains("note")) {
                System.out.println("\nYou pick up the note and read it. It says in a font long forgotten:");
                System.out.println("\"Beware the darkness ahead. Bring a light or be lost forever.\"\n");
            } 
            else if (choice.contains("i")) {  // Check inventory
                if (items > 0) {
                    System.out.println("You have " + items + " item(s).");
                    if (hasTorch) {
                        System.out.println("You have a torch.");
                    }
                } else {
                    System.out.println("You have no items yet.");
                }
            } 
            else if (choice.contains("north")) {
                if (playerX == 0 && playerY == 0 && !hasTorch) {
                    System.out.println("\nYou hesitate. The darkness ahead feels overwhelming without a light source.");
                } else {
                    playerY++; 
                    System.out.println("You step forward.");
                }
            } 
            else if (choice.contains("south")) {
                playerY--;  
                System.out.println("You head back.");
            } 
            else if (choice.contains("east")) {
                playerX++; 
                System.out.println("You move east.");
            } 
            else if (choice.contains("west")) {
                playerX--; 
                System.out.println("You move west.");
            } 
            else if (choice.contains("enter cave") || choice.contains("go north")) {
                if (hasTorch) {
                    playerY++; 
                    System.out.println("You step into the cave, a distant dripping and a musky smell meet your senses.");
                    System.out.println("A flickering torch in your hand casts shadows on the rough walls.");
                    break;
                } else {
                    System.out.println("It's too dark to proceed. You need a light.");
                }
            } 
            else if (choice.contains("go south") || choice.contains("leave")) {
                System.out.println("You walk away from the cave. Game Over.");
                break;
            } 
            else if (choice.contains("take torch")) {
                hasTorch = true;
                System.out.println("You picked up a torch. Now you can safely enter the cave.");
            } 
            else if (choice.contains("help")) {
                System.out.println("\nDirections are north, south, east, and west.");
                System.out.println("Check what you have with 'i'. Restart with 'r'.");
            } 
            else if (choice.equals("r")) {  // Restart the game
                System.out.println("Administering amnesia... Restarting.");
                return;  // Exits this function, allowing `while (true)` in `main` to restart
            } 
            else {
                System.out.println("\nInvalid command. Input 'help' for guidance.");
            }
        }
    }
}


# Objective of the game

Throughout this game you will come across various creatures including massive spiders, crocodiles and many more. Its text based and will have you fight the different monsters and solve puzzles along the way.
