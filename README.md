
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

/* -- Character sets --*/
#define LOWERCASE "abcdefghijklmnopqrstuvwxyz"
#define UPPERCASE "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
#define DIGITS    "0123456789"
#define SYMBOLS   "!@#$%^&*()-_=+[]{}|;:,.<>?"

void build_pool(char *pool, int use_upper, int use_digits, int use_symbols) {
    pool[0] = '\0';                        
    strcat(pool, LOWERCASE);              
    if (use_upper)   strcat(pool, UPPERCASE);
    if (use_digits)  strcat(pool, DIGITS);
    if (use_symbols) strcat(pool, SYMBOLS);
}

void generate_password(char *password, int length,
                       int use_upper, int use_digits, int use_symbols) {
    char pool[200];
    build_pool(pool, use_upper, use_digits, use_symbols);

    int pool_size = strlen(pool);

    for (int i = 0; i < length; i++) {

        password[i] = pool[rand() % pool_size];
    }
    password[length] = '\0';            
}

/* -- Main ---*/
int main(int argc, char *argv[]) {
    srand(time(NULL));                   

    int length      = 12;
    int use_upper   = 1;
    int use_digits  = 1;
    int use_symbols = 0;

    if (argc >= 2) length      = atoi(argv[1]);
    if (argc >= 3) use_upper   = atoi(argv[2]);
    if (argc >= 4) use_digits  = atoi(argv[3]);
    if (argc >= 5) use_symbols = atoi(argv[4]);

    if (length < 4 || length > 128) {
        printf("Error: length must be between 4 and 128.\n");
        return 1;
    }

    
    char password[129];
    generate_password(password, length, use_upper, use_digits, use_symbols);
    printf("%s\n", password);

    return 0;
}
