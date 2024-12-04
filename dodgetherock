#include <ncurses/ncurses.h>
#include <windows.h>
#include <fstream>
#include <iostream>
#include <cstdlib>
#include <ctime>
#include <unistd.h>


void menu();
void game_loop();
void view_score();
void save_high_score(int score);
int load_high_score();

int main() {
    
    initscr();
    noecho();
    curs_set(0);
    keypad(stdscr, TRUE);


 int maxY, maxX;
    getmaxyx(stdscr, maxY, maxX);  

    int centerX = maxX / 2;
    int centerY = maxY / 2;

   
    mvprintw(centerY - 1, centerX - 7, "SELAMAT DATANG!");

    
    int barX = centerX - 10;  
    int barY = centerY + 1;   

    
    for (int i = 0; i < 3; i++) {
        
        mvprintw(barY, barX, "Loading...");
        mvprintw(barY + 1, barX, "--------------------");
        mvprintw(barY + 2, barX, "|                   |");
        mvprintw(barY + 3, barX, "--------------------");

       
        for (int f = 1; f <= 18; f++) {
mvprintw(barY + 2, barX + f, "*");
            refresh();
            Sleep(150); 
        }

       
        Sleep(200);  
        clear();      
    }

    menu();

   
    endwin();
    return 0;
}

void menu() {
    int choice;
    while (true) {
        clear();
        mvprintw(5, 10, "=== DODGE THE ROCKS GAME ===");
        mvprintw(7, 12, "1. START GAME");
        mvprintw(8, 12, "2. VIEW SCORE");
        mvprintw(9, 12, "3. EXIT");
        mvprintw(11, 10, "Select an option: ");
        refresh();

        choice = getch(); 
        switch (choice) {
            case '1':
                game_loop(); 
                break;
            case '2':
                view_score(); 
                break;
            case '3':
                return; 
            default:
                mvprintw(13, 10, "Invalid choice. Press any key to try again...");
                getch();
                break;
        }
    }
}

void game_loop() {
    int max_x, max_y;
    getmaxyx(stdscr, max_y, max_x); 

    int player_x = max_x / 2; 
    int player_y = max_y - 2;
    int score = 0;
    int high_score = load_high_score();

    const int num_rocks = 3;     
    int rock_x[num_rocks], rock_y[num_rocks];
    const int rock_width = 3;   
    const int rock_height = 3;   
    int speed_delay = 50000;     

    srand(time(0)); 
    for (int i = 0; i < num_rocks; i++) {
        rock_x[i] = rand() % (max_x - rock_width);
        rock_y[i] = rand() % (max_y / 2);
    }

    nodelay(stdscr, TRUE); 
    int ch;

    while (true) {
        clear();

        
        mvprintw(0, 0, "Score: %d  High Score: %d", score, high_score);

        
        mvprintw(player_y, player_x, "O");

        
        for (int i = 0; i < num_rocks; i++) {
            
            for (int h = 0; h < rock_height; h++) {
                for (int w = 0; w < rock_width; w++) {
                    if (rock_y[i] + h < max_y && rock_x[i] + w < max_x) {
                        mvprintw(rock_y[i] + h, rock_x[i] + w, "#");
                    }
                }
            }
            rock_y[i]++;
            if (rock_y[i] >= max_y) { 
                rock_y[i] = 0;
                rock_x[i] = rand() % (max_x - rock_width);
            }

           
            if (rock_y[i] <= player_y && rock_y[i] + rock_height > player_y &&
                rock_x[i] <= player_x && rock_x[i] + rock_width > player_x) {
                if (score > high_score) save_high_score(score);
                mvprintw(max_y / 2, (max_x / 2) - 10, "GAME OVER! Press any key to return to menu...");
                nodelay(stdscr, FALSE); 
                getch();
                return; 
            }
        }

        
        ch = getch();
        switch (ch) {
            case KEY_LEFT:
                if (player_x > 0) player_x--;
                break;
            case KEY_RIGHT:
                if (player_x < max_x - 1) player_x++;
                break;
            case 'q': 
                if (score > high_score) save_high_score(score);
                return;
        }

        
        score++;

       
        if (score % 50 == 0 && speed_delay > 15000) { 
            speed_delay -= 5000; 
        }

        refresh();
        usleep(speed_delay); 
    }
}

void view_score() {
    clear();
    int high_score = load_high_score(); 
    mvprintw(5, 10, "=== HIGH SCORE ===");
    mvprintw(7, 12, "High Score: %d", high_score);
    mvprintw(9, 10, "Press any key to return to menu...");
    refresh();
    getch();
}

void save_high_score(int score) {
    std::ofstream file("high_score.txt");
    if (file.is_open()) {
        file << score; 
        file.close();
    }
}

int load_high_score() {
    std::ifstream file("high_score.txt");
    int score = 0;
    if (file.is_open()) {
        file >> score; 
        file.close();
    }
    return score;
}
