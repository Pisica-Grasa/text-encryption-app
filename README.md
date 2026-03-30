#include <stdio.h>
#include <string.h>


int countCharacters(char str[]) {
    int count = 0;
    while (str[count] != '\0') {
        count++;
    }
    return count;
}


int getDivisorVariant(int length) {
    if (length <= 100) {
        return 5;
    } else if (length <= 300) {
        return 10;
    } else if (length <= 600) {
        return 15;
    } else if (length <= 1000) {
        return 20;
    } else {
        return 1;
    }
}


void printInColumns(char str[], int length, int columns) {
    printf("\nText displayed in columns of %d:\n", columns);
    
    for (int i = 0; i < length; i++) {
        printf("%c ", str[i]);
        
     
        if ((i + 1) % columns == 0) {
            printf("\n");
        }
    }
    printf("\n"); 
}

int main() {
    char text[1001]; 
    int length;
    int divisor;

    printf("Enter the message: ");
    
    if (fgets(text, sizeof(text), stdin)) {
        text[strcspn(text, "\n")] = '\0';
        
        length = countCharacters(text);
        divisor = getDivisorVariant(length);

        printf("\nThe number of characters is: %d\n", length);
        printf("The chosen division variant is: %d\n", divisor);
        
  
        printInColumns(text, length, divisor);
    }

    return 0;
}
