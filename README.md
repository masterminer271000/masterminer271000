import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class PizzaOrder {

    private Scanner scanner;

    private ArrayList<String> recipe;
    private ArrayList<String> ingredients;
    private Map<String, String[]> ingredientsMap;

    public PizzaOrder() {
        scanner = new Scanner(System.in);
        recipe = new ArrayList<>();
        ingredients = new ArrayList<>();

        ingredients.add("pizza cheese");
        ingredients.add("diced onion");
        ingredients.add("diced green pepper");
        ingredients.add("pepperoni");
        ingredients.add("sliced mushroom");
        ingredients.add("diced jalapenos");
        ingredients.add("sardines");
        ingredients.add("pineapple chunks");
        ingredients.add("tofu");
        ingredients.add("ham chunks");
        ingredients.add("dry red pepper");
        ingredients.add("dry basil");

        ingredientsMap = new HashMap<>();
        ingredientsMap.put("pizza cheese", new String[]{"1/4 cup", "1/2 cup"});
        ingredientsMap.put("diced onion", new String[]{"1/8 cup", "1/4 cup"});
        ingredientsMap.put("diced green pepper", new String[]{"1/8 cup", "1/4 cup"});
        ingredientsMap.put("pepperoni", new String[]{"2 pieces", "4 peices", "6 peices", "8 peices"});
        ingredientsMap.put("sliced mushroom", new String[]{"1/8 cup", "1/4 cup"});
        ingredientsMap.put("diced jalapenos", new String[]{"1/8 cup", "1/4 cup"});
        ingredientsMap.put("sardines", new String[]{"1", "2", "3", "4"});
        ingredientsMap.put("pineapple chunks", new String[]{"2 pieces", "4 peices", "6 peices", "8 peices"});
        ingredientsMap.put("tofu", new String[]{"1/4 cup", "1/2 cup"});
        ingredientsMap.put("ham chunks", new String[]{"4 pieces", "8 peices", "12 peices", "16 peices"});
        ingredientsMap.put("dry red pepper", new String[]{"1 generous sprinkle", "2 generous sprinkle", "3 generous sprinkle", "4 generous sprinkle"});
        ingredientsMap.put("dry basil", new String[]{"1 generous sprinkle", "2 generous sprinkle"});

    }

    public void order() {
        System.out.println("***********************************************************************");
        System.out.println("Please choose one crust option:\n");
        System.out.printf("%-40s%-40s\n\n", "a. regular crust", "b. gluten-free crust");

        char input = getChoice("ab");

        if (input == 'a') {
            printChoice("a. regular crust = 1");
            recipe.add(String.format("%-40s%-40s", "regular crust", "1"));
        } else if (input == 'b') {
            printChoice("b. gluten-free crust = 1");
            recipe.add(String.format("%-40s%-40s", "gluten-free crust", "1"));
        }

        System.out.println("Please choose one sauce option:\n");
        System.out.printf("%-40s%-40s\n\n", "a. red sauce", "b. no red sauce");

        input = getChoice("ab");

        if (input == 'a') {
            System.out.println("Please choose one amount:\n");
            System.out.printf("%-40s%-40s\n\n", "a. 1/4 cup", "b. 1/2 cup");

            input = getChoice("ab");

            if (input == 'a') {
                printChoice("a. red sauce = 1/4 cup");
                recipe.add(String.format("%-40s%-40s", "red sauce", "1/4 cup"));
            } else if (input == 'b') {
                printChoice("a. red sauce = 1/2 cup");
                recipe.add(String.format("%-40s%-40s", "red sauce", "1/2 cup"));
            }
        } else if (input == 'b') {
            printChoice("b. no red sauce");
        }

        while (recipe.size() <= 8) {
            System.out.println("Please choose one ingredient option:\n");

            int j = 0;
            char ch = 'a';
            for (int i = 0; i < ingredients.size(); i++) {
                System.out.printf("%-40s", ch + ". " + ingredients.get(i));
                ch += 1;

                j += 1;
                if (j == 3) {
                    System.out.println();
                    j = 0;
                }
            }

            StringBuilder possibleChoice = new StringBuilder();
            char c = 'a';
            for (int i = 0; i < ingredients.size(); i++) {
                possibleChoice.append(c);
                c += 1;
            }

            System.out.println();
            int choice1 = getChoice(possibleChoice.toString()) - 'a';

            System.out.println("Please choose one amount:\n");

            String[] amounts = ingredientsMap.get(ingredients.get(choice1));
            j = 0;
            ch = 'a';
            for (int i = 0; i < amounts.length; i++) {
                System.out.printf("%-40s", ch + ". " + amounts[i]);
                ch += 1;

                j += 1;
                if (j == 3) {
                    System.out.println();
                    j = 0;
                }
            }
            System.out.println("\n");

            possibleChoice = new StringBuilder();
            c = 'a';
            for (int i = 0; i < amounts.length; i++) {
                possibleChoice.append(c);
                c += 1;
            }

            int choice2 = getChoice(possibleChoice.toString()) - 'a';

            char charchoice1 = (char) (choice1 + 'a');

            printChoice(charchoice1 + ". " + ingredients.get(choice1) + " = " + amounts[choice2]);
            recipe.add(String.format("%-40s%-40s", ingredients.get(choice1), amounts[choice2]));

            ingredients.remove(choice1);

            if (recipe.size() == 8) {
                System.out.println("Maximum ingredients amount reached.");
                break;
            }

            System.out.println("Would you like to add another ingredient?\n");
            System.out.printf("%-40s%-40s\n\n", "a. continue", "b. finished");
            choice1 = getChoice("ab");

            if (choice1 == 'b') {
                break;
            }
        }

        System.out.println("\n***********************************************************************\n");

        System.out.println("* Your pizza recipe *\n");

        for (String key : recipe) {
            System.out.println(key);
        }

        System.out.println("\n* Pizza is to be appropriately cooked until crust is cooked and toppings are fully warmed *\n");
        System.out.println("***********************************************************************");
    }

    public void printChoice(String choice) {
        System.out.printf("* You chose:     %s *\n", choice);
        System.out.println("***********************************************************************\n");
    }

    public char getChoice(String possibleChoice) {
        String c;
        do {
            System.out.print("Enter choice: ");
            c = scanner.nextLine();

            if (c.length() != 1 || !possibleChoice.contains(c)) {
                System.out.println("Wrong choice.");
            }
        } while (c.length() != 1 || !possibleChoice.contains(c));

        return c.charAt(0);
    }

    public static void main(String[] args) {
        new PizzaOrder().order();
    }
}

//You're Welcome
