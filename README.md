#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_LEN 50

struct Player {
    char name[MAX_LEN];
    int number;
    int games;
    int goals;
};

struct Player getMostGames(struct Player players[], int n) {
    struct Player maxPlayer = players[0];
    for (int i = 1; i < n; i++) {
        if (players[i].games > maxPlayer.games) {
            maxPlayer = players[i];
        }
    }
    return maxPlayer;
}

struct Player getBestAvg(struct Player players[], int n) {
    struct Player bestAvgPlayer = players[0];
    double bestAvg = (double)bestAvgPlayer.goals / bestAvgPlayer.games;
    for (int i = 1; i < n; i++) {
        double currentAvg = (double)players[i].goals / players[i].games;
        if (currentAvg > bestAvg) {
            bestAvg = currentAvg;
            bestAvgPlayer = players[i];
        }
    }
    return bestAvgPlayer;
}

int main() {
    char fileName[100];
    FILE *fp;

    printf("Enter file: ");
    scanf("%s", fileName);

    fp = fopen(fileName, "r");
    if (fp == NULL) {
        printf("Error opening file.\n");
        return 1;
    }

    int numPlayers;
    fscanf(fp, "%d", &numPlayers);

    struct Player team[numPlayers];

    for (int i = 0; i < numPlayers; i++) {
        fscanf(fp, "%s %d %d %d", team[i].name, &team[i].number, &team[i].games, &team[i].goals);
    }

    fclose(fp);

    struct Player mostGames = getMostGames(team, numPlayers);
    struct Player bestAvg = getBestAvg(team, numPlayers);

    printf("Most Games Played: %s (#%d); %d games\n", mostGames.name, mostGames.number, mostGames.games);
    printf("Best Goal Average: %s (#%d); %.2f goals\n", bestAvg.name, bestAvg.number, (double)bestAvg.goals / bestAvg.games);

    return 0;
}
