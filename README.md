# exam-

11.1
#include <stdio.h>  
#include <string.h>  
  
int main()  
{  
    char text[100], pattern[100];  
    int i, j;      int n, m;  
    int found = 0;  
  
    printf("Enter the text: ");  
    scanf("%s", text);  
  
    printf("Enter the pattern: ");  
    scanf("%s", pattern);  
  
    n = strlen(text);  
    m = strlen(pattern);  
  
    // Check every possible position      for(i = 0; i <= n - m; i++)  
    {  
        for(j = 0; j < m; j++)  
        {  
            if(text[i + j] != pattern[j])                  break;  
        }  
  
        if(j == m)  
        {  
            printf("Pattern found at position %d\n", i + 1);              found = 1;  
        }  
    }
    if(!found)  
        printf("Pattern not found.\n");  
  
    return 0;  
} 


______________________________________________

11.2 
#include <stdio.h>
#include <string.h>

#define d 256
#define q 101

void rabinKarp(char text[], char pattern[]) {
    int n = strlen(text);
    int m = strlen(pattern);
    int patternHash = 0;
    int textHash = 0;
    int h = 1;
    int found = 0;
    // Calculate h = pow(d, m-1) % q
    for (int i = 0; i < m - 1; i++) {
        h = (h * d) % q;
    }
    // Calculate initial hash values
    for (int i = 0; i < m; i++) {
        patternHash = (d * patternHash + pattern[i]) % q;
        textHash = (d * textHash + text[i]) % q;
    }
    // Slide the pattern over the text
    for (int i = 0; i <= n - m; i++) {
        // If hash values match, check characters
        if (patternHash == textHash) {
            int j;
            for (j = 0; j < m; j++) {
                if (text[i + j] != pattern[j]) {
                    break;
                }
            }
            if (j == m) {
                printf("Pattern found at index %d\n", i);
                found = 1;
            }
        }
        // Calculate hash for the next window
        if (i < n - m) {
            textHash = (d * (textHash - text[i] * h)
                        + text[i + m]) % q;
            if (textHash < 0) {
                textHash += q;
            }
        }
    }if (!found) {
        printf("Pattern not found in the text.\n");
    }
}int main() {
    char text[] = "AABAACAADAABAABA";
    char pattern[] = "AABA";
    printf("Text    : %s\n", text);
    printf("Pattern : %s\n\n", pattern);
    rabinKarp(text, pattern);

    return 0;
}
