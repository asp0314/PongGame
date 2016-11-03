# PongGame
//Aditi Patel
//11/2/2016
int x, y, w, h, speedX, speedY;
int paddleX, paddleY, paddleW, paddleH, paddleS;
boolean up, down;
color colorL= color(0, 255, 0);
int scoreL = 0;
int winScore = 10;
int currTimer=millis()+1000; 
int mi=0;//minutes
int se=0;//seconds
int circ=240;//clock radius
String dispSec="";//string var to display seconds with leading zero
float arcSt=4.71; //starting position for arc
float arcEn=0;//end position
PFont myFont;

void setup() {
  size(500, 500);
  x = width/2;
  y = height/2;
  w = 50;
  h = 50;
  speedX = 3;
  speedY = 3;
  myFont=createFont("Futura",250);
  textFont(myFont,250);
  textSize(30);
  textAlign(CENTER, CENTER);
  rectMode(CENTER);
  paddleX = 50;
  paddleY = height/2;
  paddleW = 30;
  paddleH = 100;
  paddleS = 5;
}

void draw() {
  background(0);
  textSize(20);
  text("Hi there! Let's play some pong!", 260, 375);
  timer();
  drawCircle();
  moveCircle();
  bounceOff();
  drawPaddle();
  movePaddle();
  restrictPaddle();
  contactPaddle();
  scores();
  gameOver();
} 
  
void timer() {
  if(millis()>=currTimer){ //execute when millis reaches 1 second
    currTimer=millis()+1000; //reset to one second in the future
   se=se+1; //increment seconds
   if(se==60){ //check if 60 seconds passed
     se=0; //reset to 0
     mi=mi+1; //add one minute
   }
  fill(149,175,110);
  stroke(100);
  rect(width/2-100,height/2-100,225,75); //box showing time
  //display text inside box 
  fill(255);
  if (se<=9){ //check if seconds below 10 so we can prefix with leading zero
     dispSec="0" + se; //under 10 seconds, prefix display variable with zero and append seconds 
   } else { 
     dispSec= ""+ se; //no prefix needed, seconds are above 9
   }
 textSize(60);
  text(mi + ":" + dispSec,width/2-45,height/2-40);
  }
}
  
void drawCircle() {
  fill(255, 0, 0);
  ellipse(x, y, w, h);
}

void moveCircle() {
  x = x + speedX;
  y = y + speedY;
}

void bounceOff() {
  if( x > width - w/2) {
    speedX = -speedX;
  }
  else if ( x < 0 + w/2) {
    speedX = -speedX;
    setup();
  }
   if( y > height - h/2) {
     speedY = -speedY;
   } 
  else if( y < 0 + h/2) {
    speedY = -speedY;
  }
}

void drawPaddle() {
  {
    fill(colorL);
    rect(paddleX, paddleY, paddleW, paddleH); 
  }
}

void movePaddle() {
  if(up) {
    paddleY = paddleY - paddleS;
  }
  if(down) {
    paddleY = paddleY + paddleS;
  }
}
  
void restrictPaddle() {
  if(paddleY - paddleH/2 < 0) {
    paddleY = paddleY + paddleS;
  }
  if(paddleY - paddleH/2 > height) {
    paddleY = paddleY - paddleS;
  }
}

void contactPaddle() {
  if(x - w/2 < paddleX + paddleW/2 && y + h/2 < paddleY + paddleH/2 && y - h/2 > paddleY - paddleH/2) {
  if(speedX < 0)
    speedX = -speedX;
    scoreL=scoreL+1;
  }
}

void scores() {
  fill(255);
  text(scoreL, 100, 50);
}

void gameOver() {
  if(scoreL==winScore) {
    gameOverPage("Game Over!", colorL);
  }
}

void gameOverPage(String text, color c) {

  speedX=0;
  speedY=0;
  fill(204,0, 51);
  text("GAME OVER", width/2, height/3 -40);
  fill(c);
  text("Click to play again", width/2, height/3);
  if(mousePressed) {
    scoreL=0;
    speedX=3;
    speedY=2;
  }
}


void keyPressed() {
  if(key == 'w'  || key == 'W') {
    up = true;
  }
  if(key == 's'  || key == 'S') {
    down = true;
  }
}

void keyReleased() {
   if(key == 'w'  || key == 'W') {
    up = false;
  }
  if(key == 's'  || key == 'S') {
    down = false;
  }
}
