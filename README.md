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


___________________________


// Hash Function Code // 
#include <stdio.h> 
#include <stdlib.h> 
#define TABLE_SIZE 10 
int hashTable[TABLE_SIZE]; 
// Simple modulo-based hash 
function int hash(int key) { 
return key % TABLE_SIZE; 
} 
// Initialize table with -1 (empty 
slots) void initTable() { for 
(int i = 0; i < TABLE_SIZE; 
i++) hashTable[i] = -1; 
} 
// Insert using linear 
probing void insert(int 
key) { int index = 
hash(key); 
 int start = index; 
 while (hashTable[index] != -1) { 
 index = (index + 1) % TABLE_SIZE; 
 if (index == start) { 
printf("Hash table is full!\n"); 
 return; 
 } 
 } 
 hashTable[index] = key; 
} 
// Search using linear 
probing int search(int 
key) { int index = 
hash(key); 
 int start = index; 

 while (hashTable[index] != 
-1) { if (hashTable[index] 
== key) 
 return index; 
 index = (index + 1) % TABLE_SIZE; 
 if (index == start) 
break; 
 } 
 return -1; 
} 
// Display hash table 
void display() { 
printf("\nHash 
Table:\n"); 
 for (int i = 0; i < TABLE_SIZE; i++) { 
 if (hashTable[i] != -1) 
 printf("%d --> %d\n", i, 
hashTable[i]); else 
 printf("%d --> EMPTY\n", i); 
 } 
} int 
main() { 
 initTable(); 
 insert(5); 
insert(15); 
insert(25); 
insert(35); 
insert(95); 
 display(); 
 int key = 25; 
int pos = 
search(key); 
 if (pos != -1) 
 printf("\nKey %d found at index %d\n", 
key, pos); else 
 printf("\nKey %d not found\n", key);




_____________________
6.2 prns 

#include <stdio.h>
#define N 20
#define INF 9999

int c[N][N], p[N];

int find(int x)
{
    while (p[x] != x) x = p[x];
    return x;
}

int main()
{
    int n, i, j, a, b, min, e, sum, ch;
    int v[N];

    printf("Enter vertices: ");
    scanf("%d", &n);

    printf("Enter matrix:\n");
    for (i=0;i<n;i++)
        for (j=0;j<n;j++)
            scanf("%d",&c[i][j]);

    printf("1.Prim 2.Kruskal: ");
    scanf("%d",&ch);

    for(i=0;i<n;i++) p[i]=i, v[i]=0;
    e=0; sum=0;

    if(ch==1) v[0]=1;

    while(e<n-1)
    {
        min=INF;

        for(i=0;i<n;i++)
            for(j=0;j<n;j++)
                if(c[i][j]>0 && c[i][j]<min &&
                  (ch==2 ? i<j && find(i)!=find(j)
                         : v[i] && !v[j]))
                    min=c[i][j],a=i,b=j;

        if(min==INF) break;

        printf("%d-%d : %d\n",a+1,b+1,min);
        sum+=min; e++;

        if(ch==1) v[b]=1;
        else p[find(a)]=find(b);

        c[a][b]=c[b][a]=0;
    }

    if(e==n-1) printf("MST Cost = %d\n",sum);
    else printf("Invalid choice or disconnected graph\n");

    return 0;
}





_____________
 6.1 job 

#include <stdio.h>

struct Job
{
    int id;
    int deadline;
    int profit;
};

int main()
{
    struct Job job[20], temp;
    int n, i, j, max = 0;
    int slot[20] = {0};
    int totalProfit = 0;

    printf("Enter number of jobs: ");
    scanf("%d", &n);

    for (i = 0; i < n; i++)
    {
        job[i].id = i + 1;

        printf("Enter deadline and profit for Job %d: ", i + 1);
        scanf("%d %d", &job[i].deadline,
              &job[i].profit);

        if (job[i].deadline > max)
            max = job[i].deadline;
    }

    /* Sort jobs by profit in descending order */
    for (i = 0; i < n - 1; i++)
    {
        for (j = 0; j < n - i - 1; j++)
        {
            if (job[j].profit < job[j + 1].profit)
            {
                temp = job[j];
                job[j] = job[j + 1];
                job[j + 1] = temp;
            }
        }
    }

    if (max > n)
        max = n;

    /* Schedule jobs in available slots */
    for (i = 0; i < n; i++)
    {
        for (j = job[i].deadline - 1; j >= 0; j--)
        {
            if (j < max && slot[j] == 0)
            {
                slot[j] = job[i].id;
                totalProfit += job[i].profit;
                break;
            }
        }
    }

    printf("\nSelected Jobs: ");
    for (i = 0; i < max; i++)
    {
        if (slot[i] != 0)
            printf("J%d ", slot[i]);
    }

    printf("\nMaximum Profit = %d\n", totalProfit);

    return 0;
}