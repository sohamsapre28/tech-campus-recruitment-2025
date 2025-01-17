# Source Directory
import java.io.*;
import java.nio.file.*;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeParseException;

public class ExtractLogs {

    private static final String LOG_FILE = "large_log_file.log"; // Replace with the actual log file path
    private static final String OUTPUT_DIR = "output";

    public static void main(String[] args) {
        // Check if the correct number of arguments is provided
        if (args.length != 1) {
            System.out.println("Usage: java ExtractLogs <YYYY-MM-DD>");
            return;
        }

        String dateStr = args[0];

        // Validate the date format
        if (!isValidDate(dateStr)) {
            System.out.println("Error: Invalid date format. Use YYYY-MM-DD.");
            return;
        }

        // Ensure the output directory exists
        createOutputDirectory();

        String outputFilePath = OUTPUT_DIR + File.separator + "output_" + dateStr + ".txt";

        try (BufferedReader reader = new BufferedReader(new FileReader(LOG_FILE));
             BufferedWriter writer = new BufferedWriter(new FileWriter(outputFilePath))) {

            String line;
            while ((line = reader.readLine()) != null) {
                if (line.startsWith(dateStr)) {
                    writer.write(line);
                    writer.newLine();
                }
            }

            System.out.println("Logs for " + dateStr + " have been saved to " + outputFilePath);

        } catch (FileNotFoundException e) {
            System.out.println("Error: Log file " + LOG_FILE + " not found.");
        } catch (IOException e) {
            System.out.println("Error while processing the file: " + e.getMessage());
        }
    }

    private static boolean isValidDate(String dateStr) {
        try {
            LocalDate.parse(dateStr, DateTimeFormatter.ISO_LOCAL_DATE);
            return true;
        } catch (DateTimeParseException e) {
            return false;
        }
    }

    private static void createOutputDirectory() {
        Path outputPath = Paths.get(OUTPUT_DIR);
        if (!Files.exists(outputPath)) {
            try {
                Files.createDirectories(outputPath);
            } catch (IOException e) {
                System.out.println("Error creating output directory: " + e.getMessage());
                System.exit(1);
            }
        }
    }
}
Make sure that your source code is in the `src` directory.
