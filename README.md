#include <stdio.h>
#include <string.h>
#include <conio.h>

#define MAX_CHARS 1000
#define VIEW_LIMIT 50

// 1. Function to calculate string length manually
int get_text_length(char str[]) {
    int length = 0;
    while (str[length] != '\0') {
        length++;
    }
    return length;
}

// 2. Determines grid width based on text length
int determine_divisor(int length) {
    if (length <= 100) return 5;
    if (length <= 300) return 10;
    if (length <= 600) return 15;
    if (length <= 1000) return 20;
    return 1;
}

/**
 * 3. Displays the text in a grid.
 * We must ensure the grid is a perfect rectangle for vertical reading.
 */
void display_padded_grid(char str[], int length, int columns) {
    printf("\n--- STEP 1: FORMATTED GRID (Width: %d) ---\n", columns);

    int rows = (length + columns - 1) / columns;

    for (int r = 0; r < rows; r++) {
        for (int c = 0; c < columns; c++) {
            int index = r * columns + c;
            if (index < length) {
                printf("%c ", str[index]);
            } else {
                printf(". "); // Visual padding
            }
        }
        printf("\n");
    }
    printf("------------------------------------------\n");
}

/**
 * 4. RECONSTRUCT TEXT (Vertical Reading)
 * This reads top-to-bottom, column-by-column.
 * It includes the padding spaces to keep the grid structure intact.
 */
void reconstruct_vertically(char str[], int length, int columns) {
    int rows = (length + columns - 1) / columns;
    char reconstructed[MAX_CHARS + 500]; // Buffer for reconstructed text
    int k = 0;

    printf("\n--- STEP 2: VERTICAL RECONSTRUCTION ---\n");
    printf("Reading order: Column 1 (top-down), Column 2 (top-down)... \n\n");

    for (int c = 0; c < columns; c++) {
        for (int r = 0; r < rows; r++) {
            int index = r * columns + c;

            if (index < length) {
                reconstructed[k++] = str[index];
            } else {
                reconstructed[k++] = ' '; // Fill gaps with space for reconstruction
            }
        }
    }
    reconstructed[k] = '\0'; // Null-terminate the new string

    printf("Result:\n\"%s\"\n", reconstructed);
    printf("------------------------------------------\n");
}

int main() {
    char text[MAX_CHARS + 1] = "";
    int count = 0;
    char input_char;

    printf("Type your message (MAX %d chars). Press ENTER to finish.\n", MAX_CHARS);
    printf("----------------------------------------------------------\n");

    // Real-time typing loop
    while (count < MAX_CHARS) {
        int start_index = (count > VIEW_LIMIT) ? (count - VIEW_LIMIT) : 0;

        printf("\r[Count: %4d | Left: %4d] Text: ...%s",
               count, MAX_CHARS - count, text + start_index);

        printf("   \b\b\b");

        input_char = _getch();

        if (input_char == 13) break; // Enter

        if (input_char == 8) { // Backspace
            if (count > 0) {
                count--;
                text[count] = '\0';
                printf("\r                                                                                ");
            }
        } else if (input_char >= 32 && input_char <= 126) {
            text[count++] = input_char;
            text[count] = '\0';
        }
    }

    int final_length = get_text_length(text);

    if (final_length > 0) {
        int columns = determine_divisor(final_length);

        // Execute the grid display
        display_padded_grid(text, final_length, columns);

        // Execute the vertical reconstruction
        reconstruct_vertically(text, final_length, columns);

    } else {
        printf("\n\nNo text entered. Program exiting.\n");
    }

    printf("\nPress any key to close...");
    _getch();

    return 0;
}
