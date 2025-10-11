#include <GL/glut.h>
#include <math.h>
#include <stdlib.h>
#include <ctime>
#include <vector>

// Global variables
float sunY = 500;
float angle = 0;
float trainX = 0;
float trainSpeed = 2.0f;
float carPosX = 0.0f;
float ambulancePosX = -200.0f;
float busPosX = -400.0f;
float carPosX2 = 1000.0f;
float cloudX1 = 0, cloudX2 = 300, cloudX3 = 600;
float cloudSpeed = 0.5f;
bool isRaining = false;
int currentMode = 0;
bool sirenOn = false;
float sirenTimer = 0;
bool busStopped = false;
bool thunderActive = false;
int thunderTimer = 0;
float airplaneX = -200.0f;
float airplaneSpeed = 3.0f;
int thunderBlinks = 0;
bool thunderBlinking = false;
int thunderBlinkTimer = 0;

struct Raindrop {
    float x;
    float y;
    float speed;
    float length;
};
std::vector<Raindrop> raindrops;
const float BUS_COLOR_R = 1.0f;
const float BUS_COLOR_G = 0.0f;
const float BUS_COLOR_B = 1.0f;

// Function of circle
void drawCircle_2(float cx, float cy, float r) {
    glBegin(GL_TRIANGLE_FAN);
    for (int i = 0; i <= 100; ++i) {
        float theta = 2.0f * 3.1416f * i / 100;
        float x = r * cos(theta);
        float y = r * sin(theta);
        glVertex2f(cx + x, cy + y);
    }
    glEnd();
}

// Function of rectangle
void drawRectangle_2(float x, float y, float width, float height, float r, float g, float b) {
    glColor3f(r, g, b);
    glBegin(GL_QUADS);
    glVertex2f(x, y);
    glVertex2f(x + width, y);
    glVertex2f(x + width, y + height);
    glVertex2f(x, y + height);
    glEnd();
}

// Function to draw a triangle
void drawTriangle_2(float x1, float y1, float x2, float y2, float x3, float y3, float r, float g, float b) {
    glColor3f(r, g, b);
    glBegin(GL_TRIANGLES);
    glVertex2f(x1, y1);
    glVertex2f(x2, y2);
    glVertex2f(x3, y3);
    glEnd();
}

//  sun
void drawSun_2() {
    if (currentMode == 0 || currentMode == 2) { // Day or Evening
        glColor3f(1.0, 0.7, 0.2);
        drawCircle_2(800, sunY, 40);
        glColor3f(1.0, 0.8, 0.4);
        for (int i = 0; i < 360; i += 30) {
            float rad = i * 3.1416 / 180.0;
            float x1 = 800 + cos(rad) * 50;
            float y1 = sunY + sin(rad) * 50;
            float x2 = 800 + cos(rad) * 70;
            float y2 = sunY + sin(rad) * 70;
            glBegin(GL_LINES);
            glVertex2f(x1, y1);
            glVertex2f(x2, y2);
            glEnd();
        }
    }
}

//  moon
void drawMoon_2() {
    if (currentMode == 1) { // Night
        glColor3f(0.9, 0.9, 0.8);
        drawCircle_2(800, 500, 30);
        glColor3f(0.7, 0.7, 0.6); // Craters
        drawCircle_2(790, 510, 5);
        drawCircle_2(810, 490, 4);
        drawCircle_2(800, 480, 3);
    }
}

// mountains
void drawMountains_2() {

    glColor3f(0.4, 0.35, 0.3);
    glBegin(GL_POLYGON);
    glVertex2f(150, 380);
    glVertex2f(147, 440);
    glVertex2f(195, 483.32);
    glVertex2f(266, 436);
    glVertex2f(266, 384);
    glEnd();

    glColor3f(0.4, 0.35, 0.3);
    glBegin(GL_POLYGON);
    glVertex2f(400, 400);
    glVertex2f(397, 438);
    glVertex2f(445, 499);
    glVertex2f(500, 434);
    glVertex2f(482, 403);
    glEnd();

    glColor3f(0.4, 0.35, 0.3);
    glBegin(GL_POLYGON);
    glVertex2f(727, 391);
    glVertex2f(724, 430);
    glVertex2f(760, 461);
    glVertex2f(803, 427);
    glVertex2f(805, 387);
    glEnd();

    glColor3f(0.25, 0.3, 0.2);
    glBegin(GL_POLYGON);
    glVertex2f(0, 297.378);
    glVertex2f(0, 431.207);
    glVertex2f(82.160, 521.206);
    glVertex2f(256.457, 374.875);
    glVertex2f(255.807, 292.279);
    glEnd();


    glColor3f(0.3, 0.35, 0.25);
    glBegin(GL_POLYGON);
    glVertex2f(210.807, 294.785);
    glVertex2f(213.473, 415.165);
    glVertex2f(350.058, 531.855);
    glVertex2f(490.536, 374.468);
    glVertex2f(492.278, 292.285);
    glEnd();


    glColor3f(0.35, 0.4, 0.3);
    glBegin(GL_POLYGON);
    glVertex2f(439.654, 295.068);
    glVertex2f(440.666, 427.009);
    glVertex2f(630.899, 538.027);
    glVertex2f(767.548, 416.775);
    glVertex2f(766.928, 291.998);
    glEnd();


    glColor3f(0.35, 0.4, 0.3);
    glBegin(GL_POLYGON);
    glVertex2f(757.512, 291.550);
    glVertex2f(763.690, 414.240);
    glVertex2f(892.637, 527.107);
    glVertex2f(1000, 405.440);
    glVertex2f(1000, 290.000);
    glEnd();


    glColor3ub(139, 115, 85);
    glBegin(GL_QUADS);
    glVertex2f(0, 300);
    glVertex2f(0, 100);
    glVertex2f(1000, 120);
    glVertex2f(1000, 300);
    glEnd();

    glColor3f(0.2, 0.25, 0.15);
    glBegin(GL_LINES);
    glVertex2f(300, 350); glVertex2f(350, 500);
    glVertex2f(400, 400); glVertex2f(450, 480);
    glVertex2f(600, 400); glVertex2f(650, 500);
    glEnd();
}


// river
void drawRiver_2() {
    glBegin(GL_POLYGON); // River body
    glColor3f(0.0, 0.3, 0.6); // Darker blue at bottom
    glVertex2f(0, 0);
    glVertex2f(1000, 0);
    glColor3f(0.0, 0.5, 0.8); // Lighter blue at top
    glVertex2f(1000, 120);
    glVertex2f(0, 120);
    glEnd();

    // River banks (wavy brown)
    glBegin(GL_POLYGON);
    glColor3f(0.5, 0.4, 0.2);
    glVertex2f(0, 120);
    glVertex2f(50, 125);
    glVertex2f(100, 130);
    glVertex2f(150, 122);
    glVertex2f(200, 135);
    glVertex2f(250, 120);
    glVertex2f(300, 125);
    glVertex2f(350, 120);
    glVertex2f(400, 128);
    glVertex2f(450, 122);
    glVertex2f(500, 130);
    glVertex2f(550, 120);
    glVertex2f(600, 125);
    glVertex2f(650, 120);
    glVertex2f(700, 130);
    glVertex2f(750, 125);
    glVertex2f(800, 120);
    glVertex2f(850, 130);
    glVertex2f(900, 125);
    glVertex2f(950, 120);
    glVertex2f(1000, 122);
    glVertex2f(1000, 120);
    glVertex2f(0, 120);
    glEnd();

    // River vegetation/reeds
    for (int x = 30; x < 1000; x += 80) {
        glColor3f(0.1, 0.6, 0.3);
        glBegin(GL_TRIANGLES);
        glVertex2f(x, 120);
        glVertex2f(x + 5, 130);
        glVertex2f(x + 10, 120);
        glEnd();
    }

    // Water ripples/highlights
    glColor3f(0.7, 0.8, 0.9);
    for (float x = 0; x <= 1000; x += 60) {
        glBegin(GL_LINE_STRIP);
        for (int i = 0; i <= 30; ++i) {
            float xi = x + i * 2;
            float yi = 100 + 5 * sin(xi * 0.02);
            glVertex2f(xi, yi);
        }
        glEnd();
    }
}
// background buildings
void drawBackgroundBuilding_2(float x, float y, float width, float height, float r, float g, float b) {
    // Main building body
    drawRectangle_2(x, y, width, height, r, g, b);

    // Simple windows (fewer and less detailed)
    glColor3f(currentMode == 1 ? 1.0 : r+0.3, currentMode == 1 ? 1.0 : g+0.3, currentMode == 1 ? 0.0 : b+0.3);
    for (float i = x + 5; i < x + width - 5; i += width/4) {
        for (float j = y + 5; j < y + height - 15; j += height/5) {
            drawRectangle_2(i, j, 8, 10,
                         currentMode == 1 ? 1.0 : r+0.3,
                         currentMode == 1 ? 1.0 : g+0.3,
                         currentMode == 1 ? 0.0 : b+0.3);
        }
    }

    // Simple roof
    drawRectangle_2(x-3, y+height, width+6, 5, r*0.7, g*0.7, b*0.7);
}
// residential building
void drawResidentialBuilding_2(float x, float y, float width, float height) {
    drawRectangle_2(x, y, width, height, 0.9, 0.8, 0.7); // Building body
    glColor3f(currentMode == 1 ? 1.0 : 0.4, currentMode == 1 ? 1.0 : 0.6, currentMode == 1 ? 0.0 : 0.8); // Window color changes with mode
    for (float i = x + 10; i < x + width - 10; i += 25) {
        for (float j = y + 10; j < y + height - 20; j += 30) {
            drawRectangle_2(i, j, 15, 20,
                            currentMode == 1 ? 1.0 : 0.4,
                            currentMode == 1 ? 1.0 : 0.6,
                            currentMode == 1 ? 0.0 : 0.8);
            glColor3f(0.2, 0.2, 0.2); // Window panes
            glBegin(GL_LINES);
            glVertex2f(i, j + 10);
            glVertex2f(i + 15, j + 10);
            glVertex2f(i + 7.5, j);
            glVertex2f(i + 7.5, j + 20);
            glEnd();
        }
    }
    glColor3f(0.6, 0.4, 0.2); // Roof trim
    drawRectangle_2(x - 5, y + height - 20, width + 10, 10, 0.6, 0.4, 0.2);
    glColor3f(0.7, 0.7, 0.7); // Chimney
    drawRectangle_2(x + width / 2 - 10, y + height, 20, 15, 0.7, 0.7, 0.7);
}

// Bangladeshi commercial building
void drawBangladeshiCommercial_2(float x, float y, float width, float height) {
    drawRectangle_2(x, y, width, height, 0.5, 0.5, 0.6); // Building body
    glColor3f(0.6, 0.2, 0.2); // Top decorative band
    drawRectangle_2(x, y, width, 40, 0.6, 0.2, 0.2);
    glColor3f(0.9, 0.9, 0.9); // Small rectangles on band
    for (float i = x + 5; i < x + width - 5; i += width / 3) {
        drawRectangle_2(i, y + 30, width / 3 - 5, 10, 0.9, 0.9, 0.9);
    }
    glColor3f(currentMode == 1 ? 1.0 : 0.5, currentMode == 1 ? 1.0 : 0.7, currentMode == 1 ? 0.0 : 0.9); // Windows
    for (float i = x + 10; i < x + width - 10; i += 30) {
        for (float j = y + 50; j < y + height - 10; j += 35) {
            drawRectangle_2(i, j, 20, 25,
                            currentMode == 1 ? 1.0 : 0.5,
                            currentMode == 1 ? 1.0 : 0.7,
                            currentMode == 1 ? 0.0 : 0.9);
        }
    }
    glColor3f(0.9, 0.5, 0.1); // Sign/awning area
    drawRectangle_2(x + width / 2 - 30, y + height, 60, 10, 0.9, 0.5, 0.1);
}

// a school building
void drawSchoolBuilding_2(float x, float y, float width, float height) {
    drawRectangle_2(x, y, width, height, 1.0, 0.8, 0.2); // Main building body (yellow)

    glColor3f(0.7, 0.1, 0.1); // Roof (red triangular part)
    glBegin(GL_TRIANGLES);
    glVertex2f(x - 10, y + height);
    glVertex2f(x + width + 10, y + height);
    glVertex2f(x + width / 2, y + height + 40); // Apex of the roof
    glEnd();

    drawRectangle_2(x + width / 2 - 20, y, 40, height * 0.6, 0.8, 0.8, 0.8); // Entrance area (light grey)

    glColor3f(0.1, 0.3, 0.7); // Double doors (blue)
    drawRectangle_2(x + width / 2 - 18, y, 17, height * 0.5, 0.1, 0.3, 0.7);
    drawRectangle_2(x + width / 2 + 1, y, 17, height * 0.5, 0.1, 0.3, 0.7);

    // Windows (blueish, light up at night)
    float windowColorR = currentMode == 1 ? 1.0 : 0.3;
    float windowColorG = currentMode == 1 ? 1.0 : 0.5;
    float windowColorB = currentMode == 1 ? 0.0 : 0.8;

    for (int i = 0; i < 2; ++i) { // Two rows
        for (int j = 0; j < 2; ++j) { // Two columns
            drawRectangle_2(x + 10 + j * 30, y + 10 + i * 40, 20, 30, windowColorR, windowColorG, windowColorB);
        }
    }
    for (int i = 0; i < 2; ++i) {
        for (int j = 0; j < 2; ++j) {
            drawRectangle_2(x + width - 50 + j * 30, y + 10 + i * 40, 20, 30, windowColorR, windowColorG, windowColorB);
        }
    }

    glColor3f(0.8, 0.1, 0.1); // "SCHOOL" sign background
    drawRectangle_2(x + width / 2 - 25, y + height * 0.9, 50, 15, 0.8, 0.1, 0.1);
    glColor3f(1.0, 1.0, 1.0); // White text
    glRasterPos2f(x + width / 2 - 20, y + height * 0.9 + 4);
    const char* schoolText = "SCHOOL";
    for (int i = 0; schoolText[i] != '\0'; ++i) {
        glutBitmapCharacter(GLUT_BITMAP_HELVETICA_10, schoolText[i]);
    }

    glColor3f(0.1, 0.1, 0.1); // Clock outline
    drawCircle_2(x + width / 2, y + height + 20, 10);
    glColor3f(1.0, 1.0, 1.0); // Clock face
    drawCircle_2(x + width / 2, y + height + 20, 8);
    glColor3f(0.0, 0.0, 0.0); // Clock hands
    glBegin(GL_LINES);
    glVertex2f(x + width / 2, y + height + 20);
    glVertex2f(x + width / 2, y + height + 20 + 5); // Hour hand
    glVertex2f(x + width / 2, y + height + 20);
    glVertex2f(x + width / 2 + 4, y + height + 20 - 4); // Minute hand
    glEnd();

    glColor3f(0.4, 0.4, 0.4); // Flagpole
    drawRectangle_2(x + width / 2 + 30, y + height + 20, 3, 30, 0.4, 0.4, 0.4);

    glColor3f(0.8, 0.0, 0.0); // Flag (red part)
    glBegin(GL_QUADS);
    glVertex2f(x + width / 2 + 31, y + height + 65);
    glVertex2f(x + width / 2 + 31, y + height + 50);
    glVertex2f(x + width / 2 + 31 + 15, y + height + 50);
    glVertex2f(x + width / 2 + 31 + 15, y + height + 65);
    glEnd();
}

// Bangladeshi high-rise building
void drawBangladeshiHighRise_2(float x, float y, float width, float height) {
    drawRectangle_2(x, y, width, height, 0.4, 0.5, 0.6); // Building body
    glColor3f(currentMode == 1 ? 1.0 : 0.3, currentMode == 1 ? 1.0 : 0.4, currentMode == 1 ? 0.0 : 0.7); // Windows
    for (float j = y + 10; j < y + height - 10; j += 25) {
        for (float i = x + 5; i < x + width - 5; i += 20) {
            drawRectangle_2(i, j, 15, 15,
                            currentMode == 1 ? 1.0 : 0.3,
                            currentMode == 1 ? 1.0 : 0.4,
                            currentMode == 1 ? 0.0 : 0.7);
        }
    }
    glColor3f(0.2, 0.3, 0.4); // Roof ledge
    drawRectangle_2(x - 5, y + height, width + 10, 20, 0.2, 0.3, 0.4);
    glColor3f(0.7, 0.7, 0.7); // Antenna
    glBegin(GL_LINES);
    glVertex2f(x + width / 2, y + height + 20);
    glVertex2f(x + width / 2, y + height + 40);
    glEnd();
}

// Bangladeshi store building
void drawBangladeshiStoreBuilding_2(float x, float y, float width, float height) {
    // Main building body: light reddish-brown/orange hue (238, 139, 95 in RGB).
    drawRectangle_2(x, y, width, height, 0.93333f, 0.54510f, 0.37255f);

    // Roof (Bright Orange)
    glColor3f(1.0f, 0.471f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex2f(x - 5, y + height);
    glVertex2f(x + width + 3, y + height);
    glVertex2f(x + width, y + height + 15);
    glVertex2f(x, y + height + 15);
    glEnd();

    // Door (Earthy Green)
    glColor3f(0.314f, 0.588f, 0.235f);
    drawRectangle_2(x + width / 2 - 7, y, 10, 25, 0.314f, 0.588f, 0.235f);

    // Original Window (conditional color based on currentMode)
    if (currentMode == 1) {
        // Pinkish-purple for mode 1
        glColor3f(1.0f, 0.0f, 0.588f);
        drawRectangle_2(x + width / 4, y + 20, 10, 10, 1.0f, 0.0f, 0.588f);
    } else {
        // Light blue for other modes
        glColor3f(0.588f, 0.784f, 1.0f);
        drawRectangle_2(x + width / 4, y + 20, 10, 10, 0.588f, 0.784f, 1.0f);
    }

    // Additional Windows (Soft Blue with a slightly darker shade)
    drawRectangle_2(x + width / 8, y + height / 3, 10, 10, 0.4f, 0.6f, 0.8f); // Window 1
    drawRectangle_2(x + 5 * width / 8, y + height / 3, 10, 10, 0.4f, 0.6f, 0.8f); // Window 2
}

//  tree
void drawTree_2(float x, float y) {
    drawRectangle_2(x + 12, y, 10, 40, 0.5, 0.3, 0.1); // Trunk
    glColor3f(0.1, 0.7, 0.2); // Leaves
    drawCircle_2(x + 17, y + 55, 20);
    drawCircle_2(x + 7, y + 50, 18);
    drawCircle_2(x + 27, y + 50, 18);
}

// train track on the bridge
void drawBridgeTrack_2() {
    glColor3f(0.6, 0.6, 0.6); // Main track body
    glBegin(GL_POLYGON);
    glVertex2f(0, 270);
    glVertex2f(1000, 270);
    glVertex2f(1000, 250);
    glVertex2f(0, 250);
    glEnd();
    glColor3f(0.4, 0.3, 0.2); // Railway sleepers
    for (int i = 0; i < 1000; i += 20) {
        glBegin(GL_LINES);
        glVertex2f(i, 250);
        glVertex2f(i, 270);
        glEnd();
    }
    glColor3f(0.8, 0.8, 0.8); // Rails
    glBegin(GL_LINES);
    glVertex2f(0, 268);
    glVertex2f(1000, 268);
    glVertex2f(0, 252);
    glVertex2f(1000, 252);
    glEnd();
}

// bridge structure (pillars and road surface)
void drawBridgeStructure_2() {
    glColor3f(0.4, 0.35, 0.3); // Bridge pillars
    for (int x = 0; x <= 1000; x += 120) {
        glBegin(GL_POLYGON);
        glVertex2f(x, 250);
        glVertex2f(x + 20, 250);
        glVertex2f(x + 20, 100);
        glVertex2f(x, 100);
        glEnd();
    }
    glColor3f(0.5, 0.45, 0.4); // Bridge road/surface
    glBegin(GL_QUADS);
    glVertex2f(0, 250);
    glVertex2f(1000, 250);
    glVertex2f(1000, 245);
    glVertex2f(0, 245);
    glEnd();
}

// Function to draw the bridge guard rails
void drawBridgeGuard_2() {
    glColor3f(0.3, 0.3, 0.3); // Vertical bars
    for (int x = 0; x <= 1000; x += 10) {
        glBegin(GL_LINES);
        glVertex2f(x, 270);
        glVertex2f(x, 285);
        glEnd();
    }
    glBegin(GL_LINES); // Horizontal bars
    glVertex2f(0, 285);
    glVertex2f(1000, 285);
    glVertex2f(0, 275);
    glVertex2f(1000, 275);
    glEnd();
}
//AirPlan
void drawAirplane_2() {
    float y = 450.0f;

    // Main body
    glColor3f(0.9f, 0.9f, 0.9f);
    glBegin(GL_POLYGON);
    glVertex2f(airplaneX, y);
    glVertex2f(airplaneX + 150, y);
    glVertex2f(airplaneX + 140, y + 20);
    glVertex2f(airplaneX + 10, y + 20);
    glEnd();

    // Tail
    glColor3f(0.8f, 0.8f, 0.8f);
    glBegin(GL_POLYGON);
    glVertex2f(airplaneX + 10, y + 20);
    glVertex2f(airplaneX + 40, y + 20);
    glVertex2f(airplaneX + 30, y + 50);
    glVertex2f(airplaneX + 20, y + 50);
    glEnd();

    // Tail wings
    glBegin(GL_POLYGON);
    glVertex2f(airplaneX + 20, y + 10);
    glVertex2f(airplaneX + 50, y + 10);
    glVertex2f(airplaneX + 45, y + 15);
    glVertex2f(airplaneX + 25, y + 15);
    glEnd();

    // Windows
    glColor3f(0.4f, 0.6f, 0.8f);
    for (int i = 0; i < 6; i++) {
        drawRectangle_2(airplaneX + 30 + i * 15, y + 5, 10, 5, 0.4f, 0.6f, 0.8f);
    }

    // Engine
    glColor3f(0.5f, 0.5f, 0.5f);
    drawRectangle_2(airplaneX + 110, y + 5, 20, 10, 0.5f, 0.5f, 0.5f);

    // Nose
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_TRIANGLES);
    glVertex2f(airplaneX + 140, y + 20);
    glVertex2f(airplaneX + 150, y);
    glVertex2f(airplaneX + 140, y);
    glEnd();
}

// train
void drawTrain_2() {
    float baseY = 270;
    // Train coaches
    for (int i = 0; i < 5; ++i) {
        float cx = trainX + i * 80;
        glColor3f(0.2, 0.2, 0.8); // Coach body
        glBegin(GL_POLYGON);
        glVertex2f(cx, baseY);
        glVertex2f(cx + 70, baseY);
        glVertex2f(cx + 70, baseY + 40);
        glVertex2f(cx, baseY + 40);
        glEnd();
        glColor3f(0.1, 0.1, 0.5); // Coach top
        glBegin(GL_POLYGON);
        glVertex2f(cx, baseY + 40);
        glVertex2f(cx + 70, baseY + 40);
        glVertex2f(cx + 70, baseY + 45);
        glVertex2f(cx, baseY + 45);
        glEnd();
        glColor3f(0.9, 0.9, 1.0); // Windows
        for (int j = 0; j < 3; ++j) {
            float wx = cx + 10 + j * 20;
            glBegin(GL_POLYGON);
            glVertex2f(wx, baseY + 20);
            glVertex2f(wx + 10, baseY + 20);
            glVertex2f(wx + 10, baseY + 35);
            glVertex2f(wx, baseY + 35);
            glEnd();
        }
        glColor3f(0, 0, 0); // Wheels
        drawCircle_2(cx + 15, baseY, 5);
        drawCircle_2(cx + 55, baseY, 5);
    }
    // Train engine
    float x = trainX + 5 * 80;
    glColor3f(0.7, 0.0, 0.0); // Engine body
    glBegin(GL_POLYGON);
    glVertex2f(x, baseY);
    glVertex2f(x + 70, baseY);
    glVertex2f(x + 70, baseY + 50);
    glVertex2f(x, baseY + 50);
    glEnd();
    glColor3f(0.2, 0.2, 0.2); // Chimney/exhaust
    glBegin(GL_POLYGON);
    glVertex2f(x + 50, baseY + 50);
    glVertex2f(x + 60, baseY + 50);
    glVertex2f(x + 60, baseY + 70);
    glVertex2f(x + 50, baseY + 70);
    glEnd();
    glColor3f(0.5, 0, 0); // Front triangular part
    glBegin(GL_TRIANGLES);
    glVertex2f(x + 70, baseY);
    glVertex2f(x + 90, baseY);
    glVertex2f(x + 70, baseY + 30);
    glEnd();
    glColor3f(0.9, 0.9, 1.0); // Front window
    glBegin(GL_POLYGON);
    glVertex2f(x + 5, baseY + 30);
    glVertex2f(x + 25, baseY + 30);
    glVertex2f(x + 25, baseY + 45);
    glVertex2f(x + 5, baseY + 45);
    glEnd();
    glColor3f(0, 0, 0); // Engine wheels
    drawCircle_2(x + 15, baseY, 6);
    drawCircle_2(x + 55, baseY, 6);
}


// Function to draw a cloud
void drawCloud_2(float x, float y) {
    glColor3f(0.95, 0.95, 0.95);
    drawCircle_2(x, y, 15);
    drawCircle_2(x + 20, y, 15);
    drawCircle_2(x + 10, y + 10, 15);
}

// Function to draw the road
void drawRoad_2() {
    drawRectangle_2(0, 0, 1000, 70, 0.15, 0.15, 0.15); // Main road surface
    glColor3f(1, 1, 0.8); // Lane markers
    for (int i = 0; i < 1000; i += 40) {
        glBegin(GL_LINES);
        glVertex2f(i, 35);
        glVertex2f(i + 10, 35);
        glEnd();
    }
    glColor3ub(193,155,107); // Sidewalk/shoulder
    glBegin(GL_QUADS);
    glVertex2f(0, 150);
    glVertex2f(1000, 150);
    glVertex2f(1000, 140);
    glVertex2f(0, 140);
    glEnd();
}

// Function to draw a car
void drawCar_2(float x, float y, float r, float g, float b) {
    glColor3f(r, g, b); // Car body
    glBegin(GL_POLYGON);
    glVertex2f(x, y);
    glVertex2f(x + 80, y);
    glVertex2f(x + 80, y + 30);
    glVertex2f(x, y + 30);
    glEnd();

    glColor3f(r * 0.7f, g * 0.7f, b * 0.7f); // Car cabin
    glBegin(GL_POLYGON);
    glVertex2f(x + 15, y + 30);
    glVertex2f(x + 65, y + 30);
    glVertex2f(x + 55, y + 50);
    glVertex2f(x + 25, y + 50);
    glEnd();

    // Wheels
    glColor3f(0.05f, 0.05f, 0.05f);
    float radius = 10.0f;
    int triangleAmount = 20;
    GLfloat twicePi = 2.0f * 3.1416f;

    // First wheel
    glBegin(GL_TRIANGLE_FAN);
    glVertex2f(x + 20, y);
    for (int i = 0; i <= triangleAmount; ++i)
        glVertex2f(
            x + 20 + (radius * cos(i * twicePi / triangleAmount)),
            y + (radius * sin(i * twicePi / triangleAmount)));
    glEnd();

    // Second wheel
    glBegin(GL_TRIANGLE_FAN);
    glVertex2f(x + 60, y);
    for (int i = 0; i <= triangleAmount; ++i)
        glVertex2f(
            x + 60 + (radius * cos(i * twicePi / triangleAmount)),
            y + (radius * sin(i * twicePi / triangleAmount)));
    glEnd();
}

// Function to draw an ambulance
void drawAmbulance_2(float x, float y) {
    glColor3f(0.9, 0.1, 0.1); // Ambulance body (red)
    drawRectangle_2(x, y, 100, 35, 0.9, 0.1, 0.1);

    glColor3f(0.8, 0.05, 0.05); // Front cabin/hood
    glBegin(GL_POLYGON);
    glVertex2f(x + 70, y);
    glVertex2f(x + 100, y);
    glVertex2f(x + 100, y + 25);
    glVertex2f(x + 80, y + 35);
    glEnd();

    glColor3f(0.6, 0.8, 1.0); // Windows
    drawRectangle_2(x + 5, y + 15, 25, 15, 0.6, 0.8, 1.0);
    drawRectangle_2(x + 75, y + 10, 20, 15, 0.6, 0.8, 1.0);

    glColor3f(1.0, 1.0, 1.0); // White stripe
    drawRectangle_2(x + 5, y + 30, 90, 5, 1.0, 1.0, 1.0);
    glColor3f(1.0, 1.0, 1.0); // Red cross background
    drawRectangle_2(x + 40, y + 32, 20, 10, 1.0, 1.0, 1.0);
    glColor3f(0.9, 0.1, 0.1); // Red cross
    glBegin(GL_LINES);
    glVertex2f(x + 40, y + 37);
    glVertex2f(x + 60, y + 37);
    glVertex2f(x + 50, y + 32);
    glVertex2f(x + 50, y + 42);
    glEnd();

    glColor3f(0.05, 0.05, 0.05); // Wheels (outer)
    drawCircle_2(x + 20, y, 10);
    drawCircle_2(x + 80, y, 10);
    glColor3f(0.2, 0.2, 0.2); // Wheels (inner)
    drawCircle_2(x + 20, y, 6);
    drawCircle_2(x + 80, y, 6);

    // Siren logic (flashing light)
    sirenTimer += 0.1;
    if (sirenTimer > 0.5) {
        sirenOn = !sirenOn;
        sirenTimer = 0;
    }
    if (sirenOn) {
        glColor3f(1.0, 0.7, 0.0); // Orange/yellow siren light
        drawRectangle_2(x + 45, y + 35, 10, 10, 1.0, 0.7, 0.0);
    }
    glColor3f(1.0, 1.0, 0.7); // Headlight/taillight
    drawRectangle_2(x + 95, y + 5, 5, 5, 1.0, 1.0, 0.7);
}

// Function to draw a bus
void drawBus_2(float x, float y) {
    // Main body
    glColor3f(BUS_COLOR_R, BUS_COLOR_G, BUS_COLOR_B);
    glBegin(GL_POLYGON);
    glVertex2f(x, y);
    glVertex2f(x + 150, y);
    glVertex2f(x + 150, y + 60);
    glVertex2f(x + 140, y + 70);
    glVertex2f(x + 10, y + 70);
    glVertex2f(x, y + 60);
    glEnd();

    // Windows
    glColor3f(0.7f, 0.9f, 1.0f);
    for (int i = 0; i < 5; ++i) {
        drawRectangle_2(x + 15 + i * 25, y + 40, 20, 20, 0.7f, 0.9f, 1.0f);
    }

    // Front window
    glBegin(GL_POLYGON);
    glVertex2f(x + 140, y + 70);
    glVertex2f(x + 150, y + 60);
    glVertex2f(x + 150, y + 50);
    glVertex2f(x + 140, y + 50);
    glEnd();

    // Side stripe
    glColor3f(1.0f, 1.0f, 0.0f);
    glBegin(GL_QUADS);
    glVertex2f(x, y + 60);
    glVertex2f(x + 150, y + 60);
    glVertex2f(x + 150, y + 55);
    glVertex2f(x, y + 55);
    glEnd();

    // Wheels
    glColor3f(0.1f, 0.1f, 0.1f);
    drawCircle_2(x + 30, y, 15);
    drawCircle_2(x + 120, y, 15);

    // Wheel hubs
    glColor3f(0.5f, 0.5f, 0.5f);
    drawCircle_2(x + 30, y, 8);
    drawCircle_2(x + 120, y, 8);

    // Headlights
    glColor3f(1.0f, 1.0f, 0.8f);
    drawRectangle_2(x + 145, y + 10, 5, 10, 1.0f, 1.0f, 0.8f);
}

// Function to initialize raindrops
void initRain_2() {
    raindrops.clear();
    for (int i = 0; i < 1000; ++i) {
        Raindrop drop;
        drop.x = rand() % 1000;
        drop.y = rand() % 600 + 150; // Start drops above visible area
        drop.speed = 5 + rand() % 10;
        drop.length = 5 + rand() % 10;
        raindrops.push_back(drop);
    }
}

// Function to draw raindrops
void drawRain_2() {
    if (!isRaining) return;
    glColor3f(0.5, 0.5, 1.0); // Blueish raindrops
    glBegin(GL_LINES);
    for (auto& drop : raindrops) {
        glVertex2f(drop.x, drop.y);
        glVertex2f(drop.x - 2, drop.y - drop.length); // Slanted drops
    }
    glEnd();
}

// Function to update raindrop positions
void updateRain_2() {
    if (!isRaining) return;
    for (auto& drop : raindrops) {
        drop.x += 1.0; // Simulate wind
        drop.y -= drop.speed;
        if (drop.y < 0) { // Reset if off screen
            drop.x = rand() % 1000;
            drop.y = rand() % 600 + 150;
            drop.speed = 5 + rand() % 10;
            drop.length = 5 + rand() % 10;
        }
    }
}

// Function to draw a hospital building
void drawHospital_2(float x, float y, float width, float height) {
    drawRectangle_2(x, y, width, height, 0.95, 0.95, 0.95); // Hospital body
    glColor3f(currentMode == 1 ? 1.0 : 0.5, currentMode == 1 ? 1.0 : 0.7, currentMode == 1 ? 0.0 : 0.9); // Windows
    for (float i = x + 10; i < x + width - 10; i += 20) {
        for (float j = y + 10; j < y + height - 10; j += 20) {
            drawRectangle_2(i, j, 10, 10,
                            currentMode == 1 ? 1.0 : 0.5,
                            currentMode == 1 ? 1.0 : 0.7,
                            currentMode == 1 ? 0.0 : 0.9);
        }
    }
    glColor3f(0.8, 0.0, 0.0); // Red cross symbol
    float crossX = x + width / 2 - 5;
    float crossY = y + height - 40;
    drawRectangle_2(crossX, crossY, 10, 30, 0.8, 0.0, 0.0);
    drawRectangle_2(crossX - 10, crossY + 10, 30, 10, 0.8, 0.0, 0.0);
}

// Function to draw a roadlight
void drawRoadlight_2(float x, float y) {
    glColor3f(0.25, 0.25, 0.25); // Pole
    drawRectangle_2(x, y, 5, 80, 0.25, 0.25, 0.25);
    drawRectangle_2(x, y + 75, 30, 5, 0.25, 0.25, 0.25); // Arm
    if (currentMode == 1 || currentMode == 2) { // Lights on at night/evening
        glColor3f(1.0, 0.9, 0.0);
        drawCircle_2(x + 30, y + 77.5, 5);
    } else { // Lights off during day
        glColor3f(0.6, 0.6, 0.6);
        drawCircle_2(x + 30, y + 77.5, 5);
    }
}

// Function to draw a park bench
void drawParkBench_2(float x, float y) {
    glColor3f(0.4f, 0.25f, 0.1f); // Legs
    drawRectangle_2(x, y, 5, 20, 0.4f, 0.25f, 0.1f);
    drawRectangle_2(x + 40, y, 5, 20, 0.4f, 0.25f, 0.1f);
    glColor3f(0.7f, 0.5f, 0.3f); // Seat
    drawRectangle_2(x - 5, y + 15, 55, 10, 0.3, 0.3, 0.3); // Back support
    drawRectangle_2(x - 5, y + 25, 55, 10, 0.7f, 0.5f, 0.3f);
    glColor3f(0.4f, 0.25f, 0.1f); // Additional back legs
    drawRectangle_2(x, y + 20, 5, 15, 0.4f, 0.25f, 0.1f);
    drawRectangle_2(x + 40, y + 20, 5, 15, 0.4f, 0.25f, 0.1f);
    glColor3f(0.7f, 0.5f, 0.3f); // Top backrest
    drawRectangle_2(x - 5, y + 35, 55, 5, 0.7f, 0.5f, 0.3f);
}

// Function to draw a bus stop
void drawBusStop_2(float x, float y) {
    float baseY = y;
    glColor3f(0.5f, 0.5f, 0.5f); // Poles
    drawRectangle_2(x, baseY, 5, 50, 0.5f, 0.5f, 0.5f);
    drawRectangle_2(x + 60, baseY, 5, 50, 0.5f, 0.5f, 0.5f);
    glColor3f(0.7f, 0.7f, 0.7f); // Roof
    drawRectangle_2(x - 10, baseY + 50, 85, 10, 0.7f, 0.7f, 0.7f);
    drawParkBench_2(x + 10, baseY + 5); // Bench at bus stop
    glColor3f(0.1f, 0.1f, 0.1f); // Text color
    glRasterPos2f(x + 10, baseY + 53);
    const char* text = "  BusStop";
    for (int i = 0; text[i] != '\0'; ++i) {
        glutBitmapCharacter(GLUT_BITMAP_HELVETICA_12, text[i]);
    }
}

// Function to draw thunder flash
void drawThunder_2() {
    if (thunderActive || thunderBlinking) {
        glColor4f(1.0f, 1.0f, 1.0f, 0.7f); // White flash with transparency
        glBegin(GL_QUADS);
        glVertex2f(0, 0);
        glVertex2f(1000, 0);
        glVertex2f(1000, 600);
        glVertex2f(0, 600);
        glEnd();
    }
}

// Main display function
void display_2() {
    // Background color based on current mode
    if (currentMode == 0) // Day
        glClearColor(0.6, 0.9, 1.0, 1.0);
    else if (currentMode == 1) // Night
        glClearColor(0.0, 0.0, 0.15, 1.0);
    else if (currentMode == 2) // Evening (sunset/sunrise)
        glClearColor(0.9, 0.6, 0.3, 1.0);
    glClear(GL_COLOR_BUFFER_BIT);

    drawSun_2();
    drawMoon_2();

    drawCloud_2(cloudX1, 500);
    drawCloud_2(cloudX2, 520);
    drawCloud_2(cloudX3, 550);
    drawThunder_2();
    drawMountains_2();
    //Background buildings (behind main buildings but in front of distant mountains)
    drawBackgroundBuilding_2(3, 200, 60, 180, 0.6, 0.5, 0.4);
    drawBackgroundBuilding_2(230, 230, 110, 190, 0.6, 0.6, 0.5);
    drawBackgroundBuilding_2(370, 200, 55, 190, 0.6, 0.5, 0.5);
    drawBackgroundBuilding_2(450, 220, 60, 170, 0.5, 0.6, 0.6);
    drawBackgroundBuilding_2(540, 210, 70, 180, 0.6, 0.6, 0.5);
    drawBackgroundBuilding_2(665, 150, 50, 185, 0.5, 0.6, 0.6);
    drawBackgroundBuilding_2(720, 150, 65, 200, 0.7, 0.5, 0.5);
    drawBackgroundBuilding_2(900, 210, 80, 180, 0.5, 0.5, 0.6);

    drawRiver_2();
    drawRoad_2();
    drawBusStop_2(510, 60);
    drawAirplane_2();
    drawResidentialBuilding_2(30, 150, 80, 290);
    drawSchoolBuilding_2(235, 150, 100, 200);
    drawBangladeshiCommercial_2(120, 150, 100, 260);
    drawBangladeshiHighRise_2(350, 150, 90, 290);
    drawResidentialBuilding_2(460, 150, 80, 210);
    drawBangladeshiCommercial_2(560, 150, 100, 260);
    drawHospital_2(790, 150, 100, 270);
    drawBangladeshiStoreBuilding_2(900, 150, 120, 160);

    // Trees
    drawTree_2(23, 150);
    drawTree_2(134, 150);
    drawTree_2(253, 150);
    drawTree_2(348, 150);
    drawTree_2(453, 150);
    drawTree_2(548, 150);
    drawTree_2(663, 150);
    drawTree_2(775, 150);
    drawTree_2(908, 150);

    drawTrain_2();
    drawBridgeTrack_2();
    drawBridgeStructure_2();
    drawBridgeGuard_2();

    drawRain_2(); // Draw raindrops if it's raining

    // Roadlights
    for (int i = 50; i < 1000; i += 150) {
        drawRoadlight_2(i, 70);
    }

    // Vehicles
    drawBus_2(busPosX, 50);
    drawCar_2(carPosX, 50, 0.8f, 0.2f, 0.2f);
    drawAmbulance_2(ambulancePosX, 50);
    drawCar_2(carPosX2, 10, 0.1f, 0.4f, 0.7f);

    drawParkBench_2(900, 140); // Another park bench example

    glutSwapBuffers();
}

// Animation update function
void update_2(int value) {
    angle += 0.05f; // For general animation/rotation (if used)

    // Vehicle movement
    trainX += trainSpeed;
    if (trainX > 1000)
        trainX = -600;

    carPosX += 5.0f;
    if (carPosX > 1000)
        carPosX = -100;

    ambulancePosX += 3.0f;
    if (ambulancePosX > 1000)
        ambulancePosX = -150;

    if (!busStopped) {
        busPosX += 2.0f;
        if (busPosX > 1000)
            busPosX = -200;
    }

    carPosX2 -= 4.0f;
    if (carPosX2 < -100)
        carPosX2 = 1000;
    airplaneX += airplaneSpeed;
    if (airplaneX  >1200)
        airplaneX = -200;


    // Cloud movement
    cloudX1 -= cloudSpeed;
    cloudX2 -= cloudSpeed;
    cloudX3 -= cloudSpeed;
    if (cloudX1 < -400)
        cloudX1 = 1000;
    if (cloudX2 < -400)
        cloudX2 = 1000;
    if (cloudX3 < -400)
        cloudX3 = 1000;

    updateRain_2(); // Update raindrop positions

// Thunder logic
if (thunderActive) {
    if (!thunderBlinking) {
        thunderBlinking = true;
        thunderBlinks = 0;
        thunderBlinkTimer = 5; // Short duration for each blink
    }

    if (thunderBlinking) {
        thunderBlinkTimer--;
        if (thunderBlinkTimer <= 0) {
            thunderBlinks++;
            if (thunderBlinks >= 6) { // 3 blinks (on-off counts as 2)
                thunderActive = false;
                thunderBlinking = false;
            } else {
                // Toggle visibility for blinking effect
                thunderBlinkTimer = (thunderBlinks % 2 == 0) ? 10 : 5;
            }
        }
    }
}
    glutPostRedisplay(); // Request redraw
    glutTimerFunc(16, update_2, 0); // ~60 frames per second
}

// Keyboard input handler
void handleKeys_2(unsigned char key, int x, int y) {
    if (key == 'r' || key == 'R') {
        isRaining = !isRaining;
        if (isRaining)
            initRain_2(); // Initialize raindrops when rain starts
    } else if (key == 'd' || key == 'D')
        currentMode = 0; // Day mode
    else if (key == 'n' || key == 'N')
        currentMode = 1; // Night mode
    else if (key == 'e' || key == 'E')
        currentMode = 2; // Evening mode
    else if (key == 's' || key == 'S') {
        busStopped = !busStopped; // Toggle bus stop
    }
    else if (key == 'a' || key == 'A') {
        // Speed up airplane
        airplaneSpeed += 0.5f;
        if (airplaneSpeed > 10.0f)
            airplaneSpeed = 10.0f;
    } else if (key == 'z' || key == 'Z') {
        // Slow down airplane
        airplaneSpeed -= 0.5f;
        if (airplaneSpeed < 0.5f)
            airplaneSpeed = 0.5f;
    }

    glutPostRedisplay();
}

// Mouse input handler
void mouse_2(int button, int state, int x, int y) {
    if (button == GLUT_LEFT_BUTTON && state == GLUT_DOWN) {
        trainSpeed -= 0.5f;
        if (trainSpeed < 0.0f)
            trainSpeed = 0.0f;
    } else if (button == GLUT_RIGHT_BUTTON && state == GLUT_DOWN) {
        trainSpeed += 0.5f;
        if (trainSpeed > 100.0f)
            trainSpeed = 100.0f;
    } else if (button == GLUT_MIDDLE_BUTTON && state == GLUT_DOWN) {
        thunderActive = true;
        thunderBlinks = 0; // Reset blink counter
    }
    glutPostRedisplay();
}

// Initialization function
void init_2() {
    glClearColor(0.6, 0.9, 1.0, 1.0); // Default clear color
    gluOrtho2D(0, 1000, 0, 600);
    srand(time(0));
    initRain_2();
    glEnable(GL_BLEND);
    glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA);
}

// Main function
int main(int argc, char** argv) {
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB);
    glutInitWindowSize(1000, 600);
    glutCreateWindow("A journey by Train");
    init_2();
    glutDisplayFunc(display_2);
    glutTimerFunc(0, update_2, 0);
    glutKeyboardFunc(handleKeys_2);
    glutMouseFunc(mouse_2);
    glutMainLoop();
    return 0;
}

