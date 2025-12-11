
import java.io.*;
import java.util.*;

public class WordCounter {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("===== WORD COUNTER PROGRAM =====");
        System.out.println("1. Enter text manually");
        System.out.println("2. Read text from file");
        System.out.print("Choose an option: ");
        int choice = scanner.nextInt();
        scanner.nextLine(); // consume newline

        String text = "";

        try {
            if (choice == 1) {
                System.out.println("Enter text below:");
                text = scanner.nextLine();
            } else if (choice == 2) {
                System.out.print("Enter file name (e.g., input.txt): ");
                String filename = scanner.nextLine();
                text = readFile(filename);
            } else {
                System.out.println("Invalid choice.");
                return;
            }
        } catch (IOException e) {
            System.out.println("Error reading file.");
            return;
        }

        // Display results
        System.out.println("\n===== RESULTS =====");
        System.out.println("Word Count: " + countWords(text));
        System.out.println("Character Count: " + text.length());
        System.out.println("Sentence Count: " + countSentences(text));

        Map<String, Integer> freq = wordFrequency(text);
        System.out.println("\nTop 5 Most Frequent Words:");
        freq.entrySet()
                .stream()
                .sorted(Map.Entry.<String, Integer>comparingByValue().reversed())
                .limit(5)
                .forEach(entry -> System.out.println(entry.getKey() + ": " + entry.getValue()));
    }

    // Read file content
    public static String readFile(String filename) throws IOException {
        StringBuilder content = new StringBuilder();
        BufferedReader br = new BufferedReader(new FileReader(filename));
        String line;

        while ((line = br.readLine()) != null) {
            content.append(line).append(" ");
        }

        br.close();
        return content.toString();
    }

    // Count words
    public static int countWords(String text) {
        if (text == null || text.trim().isEmpty()) return 0;
        String[] words = text.trim().split("\\s+");
        return words.length;
    }

    // Count sentences
    public static int countSentences(String text) {
        if (text == null || text.trim().isEmpty()) return 0;
        String[] sentences = text.split("[.!?]+");
        return sentences.length;
    }

    // Word frequency map
    public static Map<String, Integer> wordFrequency(String text) {
        Map<String, Integer> map = new HashMap<>();
        String cleaned = text.toLowerCase().replaceAll("[^a-zA-Z ]", " ");
        String[] words = cleaned.split("\\s+");

        for (String w : words) {
            if (!w.isEmpty()) {
                map.put(w, map.getOrDefault(w, 0) + 1);
            }
        }
        return map;
    }
}
