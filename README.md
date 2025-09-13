## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

char keyTable[SIZE][SIZE];
int posRow[26], posCol[26]; 

void generateKeyTable(char key[]) {
    int i, j, k;
    int seen[26] = {0};
    int len = strlen(key);
    int row = 0, col = 0;

    for (i = 0; i < len; i++) {
        if (key[i] == 'j') key[i] = 'i';
        if (!isalpha(key[i])) continue; 
        key[i] = tolower(key[i]);
    }


    for (i = 0; i < len; i++) {
        int idx = key[i] - 'a';
        if (!seen[idx]) {
            keyTable[row][col] = key[i];
            posRow[idx] = row;
            posCol[idx] = col;
            seen[idx] = 1;

            col++;
            if (col == SIZE) {
                col = 0;
                row++;
            }
        }
    }

 
    for (i = 0; i < 26; i++) {
        if (i + 'a' == 'j') continue; 
        if (!seen[i]) {
            keyTable[row][col] = i + 'a';
            posRow[i] = row;
            posCol[i] = col;
            seen[i] = 1;

            col++;
            if (col == SIZE) {
                col = 0;
                row++;
            }
        }
    }
}


int prepareText(char text[], char out[]) {
    int i, j = 0;
    int len = strlen(text);

    for (i = 0; i < len; i++) {
        if (!isalpha(text[i])) continue; 
        char ch = tolower(text[i]);
        if (ch == 'j') ch = 'i'; i
        out[j++] = ch;
    }

    out[j] = '\0';
    len = j;


    char prepared[200];
    int k = 0;
    for (i = 0; i < len; i++) {
        prepared[k++] = out[i];
        if (i + 1 < len && out[i] == out[i + 1]) {
            prepared[k++] = 'x';
        }
    }
    if (k % 2 != 0) {
        prepared[k++] = 'x'; 
    }
    prepared[k] = '\0';

    strcpy(out, prepared);
    return k;
}

void encryptDigram(char a, char b, char *c1, char *c2) {
    int row1 = posRow[a - 'a'], col1 = posCol[a - 'a'];
    int row2 = posRow[b - 'a'], col2 = posCol[b - 'a'];

    if (row1 == row2) {
        *c1 = keyTable[row1][(col1 + 1) % SIZE];
        *c2 = keyTable[row2][(col2 + 1) % SIZE];
    } else if (col1 == col2) {
        *c1 = keyTable[(row1 + 1) % SIZE][col1];
        *c2 = keyTable[(row2 + 1) % SIZE][col2];
    } else {
        *c1 = keyTable[row1][col2];
        *c2 = keyTable[row2][col1];
    }
}

void encrypt(char plaintext[], char ciphertext[]) {
    int len = strlen(plaintext);
    int i, j = 0;
    for (i = 0; i < len; i += 2) {
        char c1, c2;
        encryptDigram(plaintext[i], plaintext[i + 1], &c1, &c2);
        ciphertext[j++] = c1;
        ciphertext[j++] = c2;
    }
    ciphertext[j] = '\0';
}

int main() {
    char key[100], plaintext[200], prepared[200], ciphertext[200];

    printf("Enter the key: ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = '\0'; 

    printf("Enter plaintext: ");
    fgets(plaintext, sizeof(plaintext), stdin);
    plaintext[strcspn(plaintext, "\n")] = '\0';

    generateKeyTable(key);
    int len = prepareText(plaintext, prepared);
    encrypt(prepared, ciphertext);

    printf("\n--- Playfair Cipher ---\n");
    printf("Key Table:\n");
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            printf("%c ", keyTable[i][j]);
        }
        printf("\n");
    }
    printf("\nPrepared Text: %s\n", prepared);
    printf("Cipher Text: %s\n", ciphertext);

    return 0;
}
```




Output:
<img width="1537" height="874" alt="Screenshot 2025-09-13 at 2 24 53 PM" src="https://github.com/user-attachments/assets/040f23f5-5af2-4b78-95d9-114d31638670" />
