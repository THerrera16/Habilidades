#include <stdio.h>
#include <stdlib.h>

int compare(const void *a, const void *b) {
    return (*(int *)a - *(int *)b);
}


typedef struct {
    int skill;
    int originalIndex;
} Player;

int comparePlayer(const void *a, const void *b) {
    Player *playerA = (Player *)a;
    Player *playerB = (Player *)b;
    return (playerA->skill - playerB->skill);
}
long long dividePlayers(Player* players, int skillSize) {
    if (skillSize % 2 != 0) {
        return -1;
    }
    qsort(players, skillSize, sizeof(Player), comparePlayer);
    int n = skillSize / 2;
    int targetSum = players[0].skill + players[skillSize - 1].skill;
    printf("\nEquipos formados (con suma de habilidad %d):\n", targetSum);
    int i;
    for ( i = 0; i < n; i++) {
        int first = i;
        int last = skillSize - 1 - i;
        if (players[first].skill + players[last].skill != targetSum) {
            printf("No es posible formar equipos con igual suma de habilidad.\n");
            return -1;
        }
        printf("Equipo %d: Jugador %d (habilidad: %d) y Jugador %d (habilidad: %d), Química: %lld\n", 
               i + 1, 
               players[first].originalIndex + 1, 
               players[first].skill, 
               players[last].originalIndex + 1, 
               players[last].skill,
               (long long)players[first].skill * players[last].skill);
    }
    long long totalChemistry = 0;
    for (i = 0; i < n; i++) {
        int first = i;
        int last = skillSize - 1 - i;
        long long chemistry = (long long)players[first].skill * players[last].skill;
        totalChemistry += chemistry;
    }
    return totalChemistry;
}

int main() {
    int n;
    printf("Ingrese el numero de jugadores: ");
    scanf("%d", &n);
    if (n % 2 != 0) {
        printf("El numero de jugadores debe ser par.\n");
        return 1;
    }
    Player *players = (Player *)malloc(n * sizeof(Player));
    if (players == NULL) {
        printf("Error de asignacion de memoria.\n");
        return 1;
    }
    int i;
    printf("Ingrese las habilidades de los %d jugadores--:\n", n);
    for (i = 0; i < n; i++) {
        printf("Jugador %d: ", i + 1);
        scanf("%d", &players[i].skill);
        players[i].originalIndex = i; 
    }
    long long result = dividePlayers(players, n);
    if (result != -1) {
        printf("\nSuma total de la quimica de todos los equipos: %lld\n", result);
    }
    free(players);
    return 0;
}
