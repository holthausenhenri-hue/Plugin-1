# NightfallSMPnoArmorNeth

package de.zyrex.netheriteban;

import org.bukkit.Material;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.event.inventory.CraftItemEvent;
import org.bukkit.event.inventory.PrepareItemCraftEvent;
import org.bukkit.event.inventory.PrepareSmithingEvent;
import org.bukkit.event.inventory.SmithItemEvent;
import org.bukkit.inventory.ItemStack;
import org.bukkit.plugin.java.JavaPlugin;

public final class NetheriteBan extends JavaPlugin
        implements Listener {

    @Override
    public void onEnable() {
        getServer().getPluginManager()
                .registerEvents(this, this);
        getLogger().info(
                "NetheriteBan wurde aktiviert!"
        );
    }

    private boolean isNetheriteArmor(ItemStack item) {
        if (item == null) return false;

        return switch (item.getType()) {
            case NETHERITE_HELMET,
                 NETHERITE_CHESTPLATE,
                 NETHERITE_LEGGINGS,
                 NETHERITE_BOOTS -> true;
            default -> false;
        };
    }

    @EventHandler
    public void onPrepareCraft(PrepareItemCraftEvent event) {
        if (isNetheriteArmor(event.getInventory().getResult())) {
            event.getInventory().setResult(
                    new ItemStack(Material.AIR)
            );
        }
    }

    @EventHandler
    public void onCraft(CraftItemEvent event) {
        if (isNetheriteArmor(event.getCurrentItem())) {
            event.setCancelled(true);
        }
    }

    @EventHandler
    public void onPrepareSmithing(PrepareSmithingEvent event) {
        if (isNetheriteArmor(event.getResult())) {
            event.setResult(null);
        }
    }

    @EventHandler
    public void onSmithItem(SmithItemEvent event) {
        if (isNetheriteArmor(event.getInventory().getResult())) {
            event.setCancelled(true);
        }
    }
}
