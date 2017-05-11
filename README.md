package de.soulwolf.autovote;

import org.bukkit.command.CommandExecutor;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.plugin.java.JavaPlugin;
import net.crytec.api.events.*;
import org.bukkit.Bukkit;
import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.entity.Player;

public class Votez extends JavaPlugin implements Listener, CommandExecutor {
	private static final boolean True = false;
	public void onEnable() {
		this.getCommand("votez");
		this.getCommand("votezr");
		loadConfig();
  }
	
	//Integer for Global counting
	public static class Zahlen {
		public static int votez;
	}
	
	//Counting Votes and doing stuff with it
	@EventHandler 
	public void onVote(VotifierEvent vot) {
		int vz1 = getConfig().getInt("Config.votez1");
		int vz2 = getConfig().getInt("Config.votez2");
		int vz3 = getConfig().getInt("Config.votez3");
	    Zahlen.votez = Zahlen.votez + 1;
	    System.out.println("[Votez] Aktuelle Votes: " + Zahlen.votez);
	    if(Zahlen.votez == vz1) {
	    	System.out.println("Das 1. Voteziel von " + vz1 + "  wurde erreicht!");
	    	Bukkit.getServer().dispatchCommand(Bukkit.getConsoleSender(),"treasure give all 0");
	    }
	    if(Zahlen.votez == vz2) {
	    	System.out.println("Das 2. Voteziel von " + vz2 + "  wurde erreicht!");
	    	Bukkit.getServer().dispatchCommand(Bukkit.getConsoleSender(),"treasure give all 0");
	    }
	    if(Zahlen.votez == vz3) {
	    	System.out.println("Das 3. Voteziel von " + vz3 + "  wurde erreicht!");
	    	Bukkit.getServer().dispatchCommand(Bukkit.getConsoleSender(),"treasure give all 0");
	    }
	}
	
	//Command for testing and controlling
	@Override
	public boolean onCommand(CommandSender sender, Command cmd, String label, String[] args) {
          if (cmd.getName().equalsIgnoreCase("votez")) {
        	 if(sender instanceof  Player) {       		 
        		 Player p = (Player)sender;
   		         int vz1 = getConfig().getInt("Config.votez1");
  		         int vz2 = getConfig().getInt("Config.votez2");
 		         int vz3 = getConfig().getInt("Config.votez3");
    		     p.sendMessage("Aktuelle Votes: "+Zahlen.votez+"");
    		     p.sendMessage("\nDie Voteziele lauten: \n 1. Voteziel: "+vz1+" Votes\n 2. Voteziel: "+vz2+" Votes\n 3. Voteziel: "+vz3+" Votes");
        	     System.out.println(Zahlen.votez);
        	     return true;
             }
          }
          if(cmd.getName().equalsIgnoreCase("Votezr")) {
        	  if(sender instanceof Player) {
        		  Player p = (Player)sender;
        		  if(p.hasPermission("votez.reset")) {
        			  Zahlen.votez = 0;
        			  p.sendMessage("Der Votecounter wurde resettet!");
        			  return true;
        		  }
        	  }
          }        	  
		return true;
    }
	
	//Config loader
	public void loadConfig() {
		getConfig().options().copyDefaults(True);
		saveConfig();
    }
}
