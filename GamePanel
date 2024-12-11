
/* GamePanel class acts as the main "game loop" - continuously runs the game and calls whatever needs to be called

Child of JPanel because JPanel contains methods for drawing to the screen

Implements KeyListener interface to listen for keyboard input

Implements Runnable interface to use "threading" - let the game do two things at once

*/
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;

public class GamePanel extends JPanel implements Runnable, KeyListener {

	// dimensions of window
	public static final int GAME_WIDTH = 1250;
	public static final int GAME_HEIGHT = 750;

	public Thread gameThread;
	public Image image;
	public Graphics graphics;
	public PlayerCar mainCar;
	public Ball2 fallingBall;
	public Scoreclass score;
	private Image roadImage;
	
	public GamePanel() {

		roadImage = new ImageIcon("RoadDesign.png").getImage();

		mainCar = new PlayerCar(GAME_WIDTH / 2, GAME_HEIGHT / 2); // create a player controlled ball, set start location
																// to middle of screen
		fallingBall = new Ball2((int) (Math.random() * GAME_WIDTH), 0); // create a ball, set start location to top of
																		// screen and random x val
		score = new Scoreclass(GAME_WIDTH / 2, 20); // crets a scroe class and sets the location of the text

		this.setFocusable(true); // make everything in this class appear on the screen
		this.addKeyListener(this); // start listening for keyboard input

		// add the MousePressed method from the MouseAdapter - by doing this we can
		// listen for mouse input. We do this differently from the KeyListener because
		// MouseAdapter has SEVEN mandatory methods - we only need one of them, and we
		// don't want to make 6 empty methods
		addMouseListener(new MouseAdapter() {
			public void mousePressed(MouseEvent e) {
				mainCar.mousePressed(e);
			}
		});
		this.setPreferredSize(new Dimension(GAME_WIDTH, GAME_HEIGHT));

		// make this class run at the same time as other classes (without this each
		// class would "pause" while another class runs). By using threading we can
		// remove lag, and also allows us to do features like display timers in real
		// time!
		gameThread = new Thread(this);
		gameThread.start();
	}

	// paint is a method in java.awt library that we are overriding. It is a special
	// method - it is called automatically in the background in order to update what
	// appears in the window. You NEVER call paint() yourself
	public void paint(Graphics g) {	
		
		Graphics2D g2D = (Graphics2D) g;
		
		g2D.drawImage(roadImage, 0, 0, GAME_WIDTH, GAME_HEIGHT, this);
		draw(g); // draw all game elements on top of the background
	}

	// call the draw methods in each class to update positions as things move
	public void draw(Graphics g) {
		mainCar.draw(g);
		fallingBall.draw(g);
		score.draw(g);
	}

	// call the move methods in other classes to update positions
	// this method is constantly called from run(). By doing this, movements appear
	// fluid and natural. If we take this out the movements appear sluggish and
	// laggy
	public void move() {
		mainCar.move();
		fallingBall.move();
	}

	// handles all collision detection and responds accordingly
	public void checkCollision() {

		// force player to remain on screen
		if (mainCar.y <= 0) {
			mainCar.y = 0;
		}

		if (mainCar.y >= GAME_HEIGHT - PlayerCar.BALL_DIAMETER) {
			mainCar.y = GAME_HEIGHT - PlayerCar.BALL_DIAMETER;
		}

		if (mainCar.x <= 0) {
			mainCar.x = 0;
		}

		if (mainCar.x + PlayerCar.BALL_DIAMETER >= GAME_WIDTH) {
			mainCar.x = GAME_WIDTH - PlayerCar.BALL_DIAMETER;
		}

		// force the falling object to go back to top and a random x value
		if (fallingBall.y == GAME_HEIGHT) {
			fallingBall.y = 0;
			fallingBall.x = (int) (Math.random() * 250);
			Scoreclass.score++;
		}

		if (mainCar.intersects(fallingBall)) {
			Scoreclass.score = 0;
			fallingBall.y = 0;
			fallingBall.x = (int) (Math.random() * 250);
		}

	}

	// run() method is what makes the game continue running without end. It calls
	// other methods to move objects, check for collision, and update the screen
	public void run() {
		// the CPU runs our game code too quickly - we need to slow it down! The
		// following lines of code "force" the computer to get stuck in a loop for short
		// intervals between calling other methods to update the screen.
		long lastTime = System.nanoTime();
		double amountOfTicks = 60;
		double ns = 1000000000 / amountOfTicks;
		double delta = 0;
		long now;

		while (true) { // this is the infinite game loop
			now = System.nanoTime();
			delta = delta + (now - lastTime) / ns;
			lastTime = now;

			// only move objects around and update screen if enough time has passed
			if (delta >= 1) {
				move();
				checkCollision();
				repaint();
				delta--;
			}
		}
	}

	// if a key is pressed, we'll send it over to the PlayerBall class for
	// processing
	public void keyPressed(KeyEvent e) {
		mainCar.keyPressed(e);
	}

	// if a key is released, we'll send it over to the PlayerBall class for
	// processing
	public void keyReleased(KeyEvent e) {
		mainCar.keyReleased(e);
	}

	// left empty because we don't need it; must be here because it is required to
	// be overridded by the KeyListener interface
	public void keyTyped(KeyEvent e) {

	}
}
