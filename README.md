#include <graphics.h>
#include <conio.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

// Constants
const int SCREEN_WIDTH = 640;
const int SCREEN_HEIGHT = 480;
const int CAR_WIDTH = 20;
const int CAR_HEIGHT = 40;
const int ROAD_WIDTH = 200;
const int ROAD_HEIGHT = SCREEN_HEIGHT;
const int OBSTACLE_WIDTH = 20;
const int OBSTACLE_HEIGHT = 20;

// Function to draw the car
void drawCar(int x, int y) {
    setcolor(YELLOW);
    rectangle(x, y, x + CAR_WIDTH, y + CAR_HEIGHT);
}

// Function to draw the road
void drawRoad() {
    setcolor(GREEN);
    rectangle(SCREEN_WIDTH / 2 - ROAD_WIDTH / 2, 0, SCREEN_WIDTH / 2 + ROAD_WIDTH / 2, ROAD_HEIGHT);
}

// Function to draw an obstacle
void drawObstacle(int x, int y) {
    setcolor(RED);
    rectangle(x, y, x + OBSTACLE_WIDTH, y + OBSTACLE_HEIGHT);
}

int main() {
    int gdriver = DETECT, gmode;
    initgraph(&gdriver, &gmode, "");

    // Set the screen resolution
    int screenWidth = getmaxx();
    int screenHeight = getmaxy();

    // Initialize the car's position
    int carX = screenWidth / 2 - CAR_WIDTH / 2;
    int carY = screenHeight - CAR_HEIGHT - 20;

    // Initialize the obstacle's position
    int obstacleX = rand() % (screenWidth - OBSTACLE_WIDTH);
    int obstacleY = -OBSTACLE_HEIGHT;

    // Game loop
    while (1) {
        // Clear the screen
        cleardevice();

        // Draw the road
        drawRoad();

        // Draw the car
        drawCar(carX, carY);

        // Draw the obstacle
        drawObstacle(obstacleX, obstacleY);

        // Move the obstacle
        obstacleY += 5;

        // Check for collision
        if (obstacleY + OBSTACLE_HEIGHT > carY && obstacleX + OBSTACLE_WIDTH > carX && obstacleX < carX + CAR_WIDTH) {
            printf("Game Over!\n");
            break;
        }

        // Check if the obstacle has moved off the screen
        if (obstacleY > screenHeight) {
            obstacleX = rand() % (screenWidth - OBSTACLE_WIDTH);
            obstacleY = -OBSTACLE_HEIGHT;
        }

        // Get user input
        if (kbhit()) {
            char ch = getch();
            if (ch == 'a' || ch == 'A') {
                carX -= 5;
            } else if (ch == 'd' || ch == 'D') {
                carX += 5;
            }
        }

        // Delay to control the frame rate
        delay(50);
    }

    // Close the graphics window
    closegraph();

    return 0;
}
