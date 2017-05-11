# Votez

package de.soulwolf.autovote;

import org.bukkit.command.CommandExecutor;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.plugin.java.JavaPlugin;
import net.crytec.api.events.VotifierEvent;
import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.entity.Player;

public class Votez extends JavaPlugin implements Listener, CommandExecutor {
	private static final boolean True = false;
	public void onEnable() {
		this.getCommand("votez");
		loadConfig();
  }
	public static class Zahlen {
		public static int votez;
	}
	@EventHandler 
	public void onVote(VotifierEvent vot) {
		int vz1 = getConfig().getInt("Config.votez1");
		int vz2 = getConfig().getInt("Config.votez2");
		int vz3 = getConfig().getInt("Config.votez3");
	    Zahlen.votez = Zahlen.votez + 1;
	    System.out.println("[Votez] Aktuelle Votes: " + Zahlen.votez);
	    if(Zahlen.votez == vz1) {
	    	System.out.println("Das 1. Voteziel von " + vz1 + "  wurde erreicht!");
	    }
	    if(Zahlen.votez == vz2) {
	    	System.out.println("Das 2. Voteziel von " + vz2 + "  wurde erreicht!");
	    }
	    if(Zahlen.votez == vz3) {
	    	System.out.println("Das 3. Voteziel von " + vz3 + "  wurde erreicht!");
	    }
	}
	@Override
	public boolean onCommand(CommandSender sender, Command cmd, String label, String[] args) {
          if (cmd.getName().equalsIgnoreCase("votez")) {
        	 if(sender instanceof  Player) {
        		 Player p = (Player)sender;
   		        int vz1 = getConfig().getInt("Config.votez1");
  		        int vz2 = getConfig().getInt("Config.votez2");
 		        int vz3 = getConfig().getInt("Config.votez3");
        		 if(args.length == 0) {
        			 Zahlen.votez = Zahlen.votez + 1;
    		         p.sendMessage("Aktuelle Votes: "+Zahlen.votez+"");
        	         System.out.println(Zahlen.votez);
        	    } else if (args.length == 1) {
        	    	if(args[0] == "ziele") {
        	    		p.sendMessage("Die Voteziele lauten: \n 1. Voteziel: "+vz1+" Votes\n 2. Voteziel: "+vz2+" Votes\n 3. Voteziel: "+vz3+" Votes");
        	    	}
        	    }
             }
          }
		return true;
    }
	public void loadConfig() {
		getConfig().options().copyDefaults(True);
		saveConfig();
    }
}
