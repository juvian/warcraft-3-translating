# Introduction

This is a guide intended for translating a wacraft 3 map when you are not the owner of such map (there are ways to make a map be multilanguage if you are the developer of it).

# Tools Needed
1. MPQ Editor: http://www.zezula.net/en/mpq/download.html
2. Wcstrings: http://clanufw.darkbb.com/t846-wcstrings-porting-translations-tool
3. A text editor. Recommended: sublime text 3 or notepad++

# Recommended Tools
1. A lua language sintax checker. There are plugins for sublime and notepad++ that support this

# Introduction
A warcraft map is stored in a format called MPQ, which is like a zip that has a lot of files inside it. To be able to view these files, you can use MPQ Editor. Open MPQ Editor and click on open mpq:

![Open MPQ](https://github.com/juvian/warcraft-3-translating/blob/master/open%20mpq.png)

Choose your warcraft map and confirm. Click ok, no need for any additional parameter. Your map will be loaded. Depending on the map, it will say [Read-Only] or not:

![Protected MPQ](https://github.com/juvian/warcraft-3-translating/blob/master/mpq%20protected.png)

When it says [Read-Only], it means that the map is protected. Don't bother translating because you won't be able to edit the warcraft map when it is in this mode. For such cases, you should either get in contact with map owner and get unprotected version. If thats not possible, you can try deprotecting. There are many guides about this such as: http://forum.wc3edit.net/deprotection-cheating-f64/rebuilding-an-mpq-t34876.html?hilit=rebuild

When it doesn't say [Read-Only], we are good to go. It would look like this:

![Unprotected MPQ](https://github.com/juvian/warcraft-3-translating/blob/master/mpq%20unprotected.png)

Close MPQ Editor, we just used it to check if it was read-only or not.

# Setting up a working directory

Create a folder with the name of the map such as Eden Rpg. Create a folder inside it called Wcstrings. Place wcstring files inside it. Make another folder with name of the map + its version such as Eden Rpg 3.2. Place the map file inside this folder. The structure should look like this:

Eden Rpg
  |-- Eden Rpg 3.2
         |-- Eden Rpg 3.2.w3x
  |-- Wcstrings
         |-- wcstrings.exe
         |-- wcstrings manual.txt
         |-- lua51
         |-- blacklist.txt
         
# Using wcstrings

Open command prompt (cmd). A quick way is using windows key + r:

![Cmd](https://github.com/juvian/warcraft-3-translating/blob/master/cmd.png)

Another way is searching for Command prompt:

![Cmd](https://github.com/juvian/warcraft-3-translating/blob/master/cmd2.png)

Now you should run the following command (run = type and then press enter):

cd path_to_your_map_folder

![Cd](https://github.com/juvian/warcraft-3-translating/blob/master/cd.png)

To run a wcstrings command, you need to put path to wcstring and then the command. Considering that wcstrings folder was made inside working directory, we can do: ../wcstrings/wcstrings.exe command

# Translating a map from scratch

Run the command path_to_wcstrings.exe a map_name map_name map_name any_word. Considering I have the map korean 2.9a_un.w3x inside path_to_your_map_folder, an example would be:

"../wcstrings/wcstrings.exe" a "korean 2.9a_un.w3x" "korean 2.9a_un.w3x" "korean 2.9a_un.w3x" eden

![All](https://github.com/juvian/warcraft-3-translating/blob/master/all.png)

Also run the command path_to_wcstrings.exe e map_name any_word. An example would be

"../wcstrings/wcstrings.exe" e eden

After running these command successfully, your folder should now have several additional files, with the any_word used as prefix:

![After All](https://github.com/juvian/warcraft-3-translating/blob/master/after%20all.png)

Ignore the files that have the _src_. You will need to translate all the other files. You can use any text editor to open these, although they need to support the characters of the language you want to translate from. Sublime or notepad++ work.

# Translating a map from previous translation

In this case, we already translated a map but a new version of it got released and we now need to make a new translated map for this new version. That is, we have available the old non translated map, the old translated map and the new non translated map.

Run the command path_to_wcstrings.exe a old_untranslated_map old_translated_map new_untranslated_map any_word. 

An example would be "../wcstrings/wcstrings.exe" a "korean 2.8c_un.w3x" "eng 2.8c_un.w3x" "korean 2.9a_un.w3x" eden

This will generate the same files as translating from scratch, but it will retain old translations. The only places where translations will be needed is where original text changed.

Run the command path_to_wcstrings.exe g old_untranslated_map old_translated_map any_word. 

An example would be "../../wcstrings/wcstrings.exe" g "korean 2.8c_un.w3x" "eng 2.8c_un.w3x" eden

This will generate the eden_strings.lua and eden_script.lua files. An alternative to this is copy and paste the files we translated on the previous version, but if you deleted them this command can create them.

Now we need to add to those files the new stuff from new version, to do that run command path_to_wcstrings.exe u new_unstranslated_map previous_used_word. An example would be

"../../wcstrings/wcstrings.exe" u "korean 2.9a_un.w3x" eden

## Finding changes (stuff that needs new traslating)

Run the command path_to_wcstrings.exe c old_untranslated_map new_untranslated_map > file_name. An example would be

"../../wcstrings/wcstrings.exe" c "korean 2.8c_un.w3x" "korean 2.9a_un.w3x"  > changes.txt

This will create a file named changes.txt with all the changes between the 2 map versions. Note that this does not check for only string changes, so there might be a change about a skill cooldown time and we will get notified of it, but its text has not changed so we don't need to translate it. Still, we will have to go over all of these changes in case one of those was a text change.

If you open changes.txt, you will see something like this: [changes.txt](changes.txt)

There will be a section for each type of changes (units, abilities, items, buffs, upgrades). For each of those sections, you will need to open the corresponding file (eden_tl_units.lua, eden_tl_abilities.lua, eden_tl_items.lua, eden_tl_buffs.lua, eden_tl_upgrades.lua) and for each line that says Object [xxxx] was changed, you will need to search in the translation file for xxxx and check if any string related to it is now in the original language instead of the translated language.

For example, it may say Object [A0D0] was changed in abilities section, and when I go to eden_tl_abilities.lua and search for A0D0, I can see it is now in korean instead of english:

[abilities](https://github.com/juvian/warcraft-3-translating/blob/master/abilities.png)

So for each change, you will need to check if it now needs retranslating.

For all messages of type Object [xxxx] was added, it means its a new ability/item/whatever so it will definitely need translating. However, new stuff is always at the end of the file so its easy to find them.

Unfortunately there is no way of checking with wcstrings what might have changed in eden_script.lua and eden_strings.lua files. A good way might be using a diff checker such as https://www.diffchecker.com/ and compare the file before using the path_to_wcstrings.exe u new_unstranslated_map previous_used_word command against the new one


# eden_strings.lua and eden_script.lua file format
A big list of lines such as 

t["플레이어 1"] = "플레이어 1";

What you need to change (translate) is only the right part of the =, as the left is the original text. So in this case, I would change that line to:

t["플레이어 1"] = "Player 1";


Be aware that many lines would not need translating, such as 

t["Objects\\\\InventoryItems\\\\TreasureChest\\\\treasurechest.mdl"] = "Objects\\\\InventoryItems\\\\TreasureChest\\\\treasurechest.mdl";

So leave those as they are. The script file contains strings found in map code (war3map.j) while strings file is from strings file (war3map.wts)

# eden_tl_abilities.lua, eden_tl_buffs.lua, eden_tl_items.lua, eden_tl_units.lua and eden_tl_upgrades.lua

Similar to the others, but these will also have lines starting with --. Those lines are just comments, it does not matter if you leave them there or remove them.

Intead of having the t[original_text] = translated_text format, you will just see things in the original language and you just have to replace it with your translation. If you lost/forgot what was the original text, you can search it in the _src_ files.

Abilities file contains ability related strings, buffs contains buff related strings, items contains item related strings, units contains unit related strings an upgrades contain upgrade related strings

# Applying translations to map

Once you have finished translating what you needed, we have to put them back into the map. It is highly recommended to make a copy of the original map and work on the new copy in case something goes wrong.

Run the command path_to_wcstrings.exe j map_copy_name word_we_used_before

In this example, it will be "../wcstrings/wcstrings.exe" j "korean 2.9a_un - Copy.w3x" eden

![Import j](https://github.com/juvian/warcraft-3-translating/blob/master/import%20j.png)

If successful, this will import both eden_script.lua and eden_strings.lua translations into the map. It will also create inside our folder the modified files of war3map.j and war3map.wts:

![Import j files](https://github.com/juvian/warcraft-3-translating/blob/master/import%20j%20files.png)

Wcstrings already replaced those files in the map, so we don't really need them but if for some reason importing to map failed, they are useful and can be manually added with MPQ Editor. Should not be normally necessary

Run the command path_to_wcstrings.exe x map_copy_name word_we_used_before

In this example, it will be "../wcstrings/wcstrings.exe" x "korean 2.9a_un - Copy.w3x" eden

![Import x](https://github.com/juvian/warcraft-3-translating/blob/master/import%20x.png)

If successful, this will import eden_tl_abilities.lua, eden_tl_buffs.lua, eden_tl_items.lua, eden_tl_units.lua and eden_tl_upgrades.lua translations into the map. It will also create inside our folder the modified files of war3map.w3a, war3map.w3h, war3map.w3q, war3map.w3t, war3map.w3u and war3map.wts:

![Import x files](https://github.com/juvian/warcraft-3-translating/blob/master/import%20x%20files.png)

Same as before, Wcstrings should have already replaced those files in the map.

You can now try out the map!

# I Have tried the translated map but all of my items were left untranslated, although all abilities were indeed translated

This means that you have made a mistake in translating the items file, so wcstrings couldn't parse it correctly and was ignored. Without a lua sintax checker, finding this is almost impossible. Run the lua sintax checker on the file that is giving problems and check if there is any mistake mentioned

         

