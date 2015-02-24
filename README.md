# Assignment-1.5

//Citation - Daniel Shiffman - nature of Code - Chapters 1 and 2
//Toxilabs - 2D attracor

import toxi.geom.*;
import toxi.physics.*;
import toxi.physics.behaviors.*;
import controlP5.*;
 
 
 //Step 1 - Create a physics environment
 // Step 2, generate random particles 
 // Step 3 - One point will be a central attrator, from the origin
 // Step 4 - another set of particles will be rotated and translated around this point, 
 
int N = 5000;                   //initial number of particle points
float ang = 0;
float F = 0.1;                //sets the drag
 
ControlP5 cp5; 
VerletPhysics physics;         // this is a 3D equivalent of Box2D
AttractionBehavior attractor;// Attractor Physics
Vec3D mousePos;

void setup() {
  colorMode(HSB);
  size(600, 600, P3D);
  background(0);
  //cp5 = new ControlP5(this);
  
  // cp5.addSlider("N")
     //.setPosition(100,50)
     //.setRange(0,255)
     //;
    //cp5.getController("N").getValueLabel().align(ControlP5.LEFT, ControlP5.BOTTOM_OUTSIDE).setPaddingX(0); 
   
  
    physics = new VerletPhysics(); // this is a 3dimensional verlet particles 
    physics.setDrag(  F );        // this is the force with wich the particles are 
                                   //attracted to their own centre, or prodution start
  }
 
void addParticle() {
  VerletParticle p = new VerletParticle(Vec3D.randomVector()); // the verlet particle needs an inttial location
                                                               // initialize the physics engien to add particles
  physics.addParticle(p);
}
 
 
void draw() {
 
  camera(10, 10*sin(radians(ang)), 80*cos(radians(ang))+width/3+mouseY/2,
  0.0, 0.0, 0.0,
  0.0, 0.0, 1.0);
  background(0);
  noFill();
   
 
    Vec3D vec = new Vec3D(0,0,0);               // storing the  vector values of the start location
 
    if (physics.particles.size() < N) {         //add particle if number of particles less than n, component
                                                //can also be used to velocity,force etc
      addParticle(  );
    }
    
     
     
     
     // attractor setup of toxilibs
     
    attractor = new AttractionBehavior(vec, 10, 10); //apply the attraction force from the attractor for the said verlet articles
                                                     // this is the definition of how we add gravity
    physics.addBehavior(attractor);
    physics.update();
    for (VerletParticle p : physics.particles) {
       
      stroke(200,50);
      point(random(width),random(height),random(width+height));
      
      // this is the attractor
      //the distance between the attractor and the mover is not constrained by pvector anymore
      
      
      pushMatrix(); 
      stroke(10, 200, 200, 50);
      translate(p.x,p.y,p.z);     
      sphere(10);
      rotateX(radians(ang*40));
      line(p.x, p.y, p.z, vec.x, vec.y, vec.z); // because of verlet particles one can directly give x,y,z
      popMatrix();
      
      
      
      //this is the mover, going around the attractor
       
      pushMatrix();
      stroke(10, 200, 200, 50);
      translate(p.x+200*cos(radians(ang*5)),p.y,p.z+200*sin(radians(ang*5))); // this creates a rotation effect in a circle
      sphere(10);
      stroke(10, 200, 200, 100);
      rotateZ(radians(ang*10));
      line(p.x, p.y, p.z, vec.x, vec.y, vec.z);
      popMatrix();
       
    }
    physics.removeBehavior(attractor); 
     
  ang += 1;
}
 

//void keyPressed(){
   
 //if( key == ' '){
      //saveFrame("  #### imagen.png");
 //}
//}
