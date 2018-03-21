# Wumpas
its a game
import java.util.Scanner;
import java.io.File;
 

/**
 * Simple, text-based interface for playing "Hunt the Wumpus"
 *   @author Dave Reed
 *   @version 4/1/13
 */
public class HuntTheWumpus extends Maze
{
 
    public static void main(String[] args) throws java.io.FileNotFoundException {
        CaveMaze maze = new CaveMaze("caves.txt");

        System.out.println("HUNT THE WUMPUS:  Your mission is to explore the maze of caves");
        System.out.println("and destroy all of the wumpi (without getting yourself killed).");
        System.out.println("To move to an adjacent cave, enter 'm' and the tunnel number.");
        System.out.println("To toss a grenade into a cave, enter 't' and the tunnel number.");
        System.out.println();

        Scanner input = new Scanner(System.in);
        while (maze.stillAlive() && maze.stillWumpi()) {
            System.out.println(maze.showLocation());

            String command = input.next().trim().toLowerCase();
            char action = command.charAt(0);
            int target;
            try {
                target = Integer.parseInt(command.substring(1));
            }
            catch (java.lang.NumberFormatException e) {
                target = input.nextInt();
            }
            
            if (action == 't') {
                System.out.println(maze.toss(target));
            } else if (action == 'm') {
                System.out.println(maze.move(target));
            } else {
                System.out.println("Unrecognized command.");
            }
        }
        System.out.println("GAME OVER");
      }
}

public class Maze extends CaveContents
{
    private Cave currentCave;
  private Cave[] caves;
  private Die d;
   public CaveMaze(String filename) throws java.io.FileNotFoundException {
      Scanner infile = new Scanner(new File(filename));
      
      int numCaves = infile.nextInt();
      this.caves = new Cave[numCaves];
      
      for (int i = 0; i < numCaves; i++) {
          int num1 = infile.nextInt();
          int num2 = infile.nextInt();
          int num3 = infile.nextInt();
          int num4 = infile.nextInt();
          String name = infile.nextLine().trim();
          this.caves[num1] = new Cave(name, num1, num2, num3, num4);
      }
    
      this.currentCave = this.caves[0];
      this.currentCave.markAsVisited(); 
    }  
      public String move(int tunnel) 
       {
      this.currentCave = this.caves[this.currentCave.getAdjNumber(tunnel)];
      this.currentCave.markAsVisited();  
      return "ok";}
       public String toss(int tunnel) {
      return "not implemented.";      
  }  
  public String showLocation() {
      String message = "You are currently in " + this.currentCave.getCaveName();
      for (int i = 1; i <= 3; i++) {
	  Cave adjCave = this.caves[this.currentCave.getAdjNumber(i)];
	  if (adjCave.hasBeenVisited()) {
	      message += "\n    (" + i + ") " + adjCave.getCaveName();
	  }
	  else {
	      message += "\n    (" + i + ") unknown";
	  }
      }
      return message;
  }
  public boolean stillAlive() {
      return true;
  }

  /**
   * Reports whether there are any wumpi remaining.
   *   @return true if wumpi remaining in the maze, false otherwise
   */
  public boolean stillWumpi() {
      return true;
  }
}
public enum CaveContents {
    EMPTY, WUMPUS, BATS, PIT
}
  
      
  

 
