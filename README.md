#include <stdio.h>
#include <string.h>
#include <conio.h>

#define MAX_CHARS 1000
#define VIEW_LIMIT 50 // Limits the characters shown during typing to prevent line breaks

// Function to calculate string length manually
int get_text_length(char str[]) {
    int length = 0;
    while (str[length] != '\0') {
        length++;
    }
    return length;
}

// Logic to determine column layout based on length
int determine_divisor(int length) {
    if (length <= 100) return 5;
    if (length <= 300) return 10;
    if (length <= 600) return 15;
    if (length <= 1000) return 20;
    return 1;
}

// Displays the final text formatted in a grid
void display_grid(char str[], int length, int columns) {
    printf("\nDisplaying text in columns of %d:\n", columns);
    for (int i = 0; i < length; i++) {
        printf("%c ", str[i]);
        if ((i + 1) % columns == 0) {
            printf("\n");
        }
    }
    printf("\n");
}

int main() {
    char text[MAX_CHARS + 1] = ""; 
    int count = 0;
    char input_char;

    printf("Type your message (MAX %d chars). Press ENTER to finish.\n", MAX_CHARS);
    printf("----------------------------------------------------------\n");

    while (count < MAX_CHARS) {
        // Horizontal scroll logic: if text is too long, we only show the end of it
        int start_index = (count > VIEW_LIMIT) ? (count - VIEW_LIMIT) : 0;

        // \r moves cursor to start. %-4d ensures the numbers don't shift the text
        printf("\r[Count: %4d | Left: %4d] Text: ...%s ", 
               count, MAX_CHARS - count, text + start_index);
        
        // Clean up character artifacts if we backspaced
        printf("  "); 
        // Move cursor back to the end of the text line
        printf("\r[Count: %4d | Left: %4d] Text: ...%s", 
               count, MAX_CHARS - count, text + start_index);

        input_char = _getch();

        if (input_char == 13) break; // Enter key

        if (input_char == 8) { // Backspace key
            if (count > 0) {
                count--;
                text[count] = '\0';
                // Clear the line visually before next update
                printf("\r                                                                                ");
            }
        } else if (input_char >= 32 && input_char <= 126) { // Printable characters
            text[count++] = input_char;
            text[count] = '\0';
        }
    }

    // Final calculations
    int final_length = get_text_length(text);
    int divisor = determine_divisor(final_length);

    printf("\n\n--- SUMMARY ---");
    printf("\nFinal Length: %d characters", final_length);
    printf("\nSelected Grid Multiplier: %d", divisor);
    printf("\n------------------------------");

    if (final_length > 0) {
        display_grid(text, final_length, divisor);
    } else {
        printf("\nNo text was entered.\n");
    }

    printf("\nPress any key to exit...");
    _getch();

    return 0;
}
