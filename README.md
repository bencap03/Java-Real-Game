# Java-Real-Game
Very fun game to play with friends 
import java.awt.Canvas;
import java.awt.Color;
import java.awt.Component;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.Image;
import java.awt.Rectangle;
import java.awt.Shape;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.LinkedList;
import java.util.Random;
import sun.audio.*;

public class MyCanvas extends Canvas  implements KeyListener { 
	
	Goodguy superdad = new Goodguy(40, 400, 100, 100, "files/Homer.png");
    Badguy  Badelf = new Badguy(500,100,80,80, "files/Evil.png");	
    LinkedList badguys = new LinkedList();
    LinkedList presents = new LinkedList();
    boolean badguyDead = false;
    String msg = "The Grinch needs to try to steal the Presents and Goodguy can't let him";
    int msgX = 50;
    int msgY = 50;
    int time=20000;
    boolean gameOver = false;
    boolean start = false;
    
    Font myFont = new Font("Comic Sans", 1, 50);
    Font myFonts = new Font("Comic Sans", 1, 15);
    
	public MyCanvas() {
		this.setSize(1000,1000); //set same size as my screen
		this.addKeyListener(this);
		playIt("files/Jingle Bells original song.wav");
		   
		Random rand = new Random();
		int winwidth = this.getWidth();
		int winheight = this.getHeight();
		
		this.setBackground(Color.WHITE);
		
		for (int i = 0; i<4; i++) {
			Present ps = new Present(0, rand.nextInt(winheight), 50, 50, "files/present.png");
			presents.add(ps);
			
		}		
   }
	  

	@Override 
	public void paint(Graphics g) {
		if (!start) {
			this.setBackground(Color.BLACK);
			Present back = new Present(0, 0, 1000, 1000, "files/APRESENTS.png");
			g.drawImage(back.getImg(), back.getxCoord(), back.getyCoord(), back.getWidth(), back.getHeight(), this);
			g.setFont(myFont);
			g.setColor(Color.black);
			g.drawString("Welcome to Present Defense", 150,100);
			g.drawString("Click ENTER to Start", 230, 780);
			g.setColor(Color.BLACK);
		}
		else if (gameOver == false) {
			time--;
			Present back = new Present(0, 0, 1000, 1000, "files/background.jpg");
			g.drawImage(back.getImg(), back.getxCoord(), back.getyCoord(), back.getWidth(), back.getHeight(), this);
			this.setFont(myFont);
			g.drawString(String.valueOf(time/650), 450, 80);
			
			g.drawImage(superdad.getImg(), superdad.getxCoord(), superdad.getyCoord(),superdad.getHeight() , superdad.getWidth(),this);
			
			if (badguyDead==false) {
				g.drawImage(Badelf.getImg(), Badelf.getxCoord(), Badelf.getyCoord(),Badelf.getHeight() , Badelf.getWidth(),this);
			}
			
			for (int i = 0; i<4; i++) {
				Present ps = (Present) presents.get(i);
				Rectangle a = new Rectangle(Badelf.getxCoord(), Badelf.getyCoord(), Badelf.getWidth(), Badelf.getHeight());
				Rectangle p = new Rectangle(ps.getxCoord(), ps.getyCoord(), ps.getWidth(), ps.getHeight());
				
				if (a.intersects(p)) {
					presents.remove(ps);
				}
				else {
					g.drawImage(ps.getImg(), ps.getxCoord(), ps.getyCoord(), ps.getWidth(), ps.getHeight(), this);
				}
			//this.setFont(myFonts);	
			//g.drawString(msg, msgX, msgY);
			}
			if (time==0) {
				gameOver = true;
			}
		}else {
			g.setColor(Color.WHITE);
			g.drawString("Game Over!", 350, 400);
		}

		
		
		repaint();
			
	}

	
	@Override
	public void keyPressed(KeyEvent e) {
		// TODO Auto-generated method stub
		System.out.println(e);
		if (start) {
			superdad.moveIt(e.getKeyCode(), this.getWidth(), this.getHeight());
			Badelf.moveIt(e.getKeyCode(), this.getWidth(), this.getHeight());
			
				Rectangle r = new Rectangle(Badelf.getxCoord(),Badelf.getyCoord(),Badelf.getHeight(),Badelf.getWidth());
				if (r.contains(superdad.getxCoord(),superdad.getyCoord())){
					System.out.println("badguy hit by superdad");
					msg = "YOU DIED";
					msgX = 450;
					msgY = 480;
					this.setFont(myFont);
					Badelf.setImg("Nothing");
					badguyDead = true;
					gameOver = true;
				
				}
		}
		else {
			if (e.getKeyCode()==10) {
				start = true;
			}
		}
			
		repaint();
		
	}
	
	@Override
	public void keyTyped(KeyEvent e) {
		
		// TODO Auto-generated method stub
		
	}

	@Override
	public void keyReleased(KeyEvent e) {
		// TODO Auto-generated method stub
		
	}
	 public void playIt(String filename) {
		  try {
			   InputStream in = new FileInputStream(filename);
			   AudioStream as = new AudioStream(in);
			   AudioPlayer.player.start(as);
		} catch(IOException e) {
			   System.out.println(e);
		   }
		   
	   }
	   
}
