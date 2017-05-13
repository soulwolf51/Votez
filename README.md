package de.soulwolf.autovote;

import org.bukkit.command.CommandExecutor;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.plugin.java.JavaPlugin;
import net.crytec.api.events.*;
import java.text.SimpleDateFormat;
import java.util.Date;
import org.bukkit.Bukkit;
import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.entity.Player;

public class Votez extends JavaPlugin implements Listener, CommandExecutor {
	private static final boolean True = false;
	public void onEnable() {
		this.getCommand("votez");
		this.getCommand("votezrst");
		this.getCommand("Votezrld");
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
		SimpleDateFormat data = new SimpleDateFormat("dd.MM.yyyy");
		String Datum = data.format(new Date());
		String datc = getConfig().getString("Config.datum");
		Zahlen.votez = getConfig().getInt("Config.votes");
	    Zahlen.votez = Zahlen.votez + 1;
	    getConfig().set("Config.votes", Zahlen.votez);
	    saveConfig();
	    System.out.println("[Votez] Aktuelle Votes: " + Zahlen.votez);
	    if(datc != Datum) {
	    	getConfig().set("Config.datum", Datum);
	    	getConfig().set("Config.votes", 0);
	    	saveConfig();
	    }
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
	
	//Commands for testing and controlling
	@Override
	public boolean onCommand(CommandSender sender, Command cmd, String label, String[] args) {
          if (cmd.getName().equalsIgnoreCase("votez")) {
        	 if(sender instanceof  Player) {
        		 SimpleDateFormat data = new SimpleDateFormat("dd.MM.yyyy");
        		 String Datum = data.format(new Date());
        		 Player p = (Player)sender;
   		         int vz1 = getConfig().getInt("Config.votez1");
  		         int vz2 = getConfig().getInt("Config.votez2");
 		         int vz3 = getConfig().getInt("Config.votez3");
 				 Zahlen.votez = getConfig().getInt("Config.votes");
    		     p.sendMessage("["+Datum+"] Aktuelle Votes: "+Zahlen.votez+"");
    		     p.sendMessage("\nDie Voteziele lauten: \n 1. Voteziel: "+vz1+" Votes\n 2. Voteziel: "+vz2+" Votes\n 3. Voteziel: "+vz3+" Votes");
        	     return true;
             }
          }
          if(cmd.getName().equalsIgnoreCase("Votezrst")) {
        	  if(sender instanceof Player) {
        		  Player p = (Player)sender;
        		  if(p.hasPermission("votez.reset")) {
        			  getConfig().set("Config.votes", 0);
        			  saveConfig();
        			  reloadConfig();
        			  p.sendMessage("Der Votecounter wurde resettet!");
        			  return true;
        		  }
        	  }
          }
          if(cmd.getName().equalsIgnoreCase("Votezrld")) {
        	  if(sender instanceof Player) {
        		  Player p = (Player)sender;
        		  if(p.hasPermission("votez.reload")) {
        			  reloadConfig();
        			  p.sendMessage("[Votez] Config reloaded!");
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
