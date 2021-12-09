# *Ten Days in August '79*: The Twin Projects
The project, completed for HIST 5706 Digital History, is one half of a larger prototype of a historical role-playing game entitled *Ten Days in August '79*. The Visual Novel component, what you have hopefully now played the demo of, is the "story" component of the game. The other section of the game is a 3D representation of the City of Pompeii, rendered from GIS data and implemented in Blender, Twinmotion, and Unreal Engine. The visual novel uses art and renders from the Unreal version of the game, and showcases the most complete sections of the reconstructed city. 

This Paradata represents the second part of the information on this project, the first part is the academic write-up entitled **Digitizing the Roman Urban Experience: A Proposal**, and should be read first. 

The story takes place on Day 3 because it features the actor Lucius and the gladiator Sempronius, and the Theatre and Gladiator barracks are the most developed part of the reconstruction.

The visual novel is intended to be a stand-alone project and complete story. It has been partitioned out from the main 3D game due to the natural instability of in-development games. Visual Novels, especially those developed on the Ren'py platform, represent a stable development environment for video game storytelling. Hopefully, much of this same story will be brought into the Unreal project, but even if this does not come to pass the visual novel will stand on its own. An early build of the game and the GDD will be provided for context, not as an official part of the VN project. 

# Environment
Using the 3D model of Pompeii created from my work in *Crane: Computational Creativity*, I brought the model into Unreal Engine, using the landscaping tools to build up the environment, lighting, and foliage. This process will not be discussed in great detail, as only two of the backgrounds featured in the Visual Novel were from the Unreal Project (Theatre and Gladiator Field). It is sufficient to say that the 3D environment of Pompeii is very much a work in process, and the best (read: prettiest and least broken) renders of the 3D space will be featured in the Visual Novel component.  

# Characters
Using the models created using the Metahumans software, I then used Blender to make custom clothing for the model. These were a bit crude, and I would like to go back and improve the clothing assets at a later time. However, they work for the purposes. 

A big challenge was getting the clothing properly rigged and animated to the Metahuman model. By copying the skeleton "Weights" from the existing Metahuman clothing, I was able to get the tunic model working. After considering it, I may try copying the weights from a female dress model, since the proportions would fit better.

Next, I set up a default scene in Unreal for rendering, and used the rigs to render out expressions. Each character had between 3-5 expressions, and I found that it was easy to manipulate the character's faces using the Metahumans skeleton. I even figured out how to attach the sword model to Sempronius' hand. It gives him some extra personality. This was done by creating a "Socket" for the model placed over his palm. 

Rendering was accomplished with the use of Unreal's movie render queue function, which includes a very good preset for high-quality still images. Some tweaking of sizes and removing the grey backdrop was required, accomplished in Clip Studio paint. Overall, the character sprites are fairly realistic and expressive. I'm happy with the outcome. 

# Ren'Py Mechanics
## Coding
The parent-child relationship between lines is much stronger than in C#. Indentation means a lot, apparently. I guess it serves a similar purpose as a terminator for a line, like **;** does in the languages I'm more familiar with. Some of the naming conventions of Python is a bit odd, since it doesn't seem to like capitalization that much, except for "True" and "False", apparently. 

Small issues aside, I found using the Ren'py scripting language very intuitive once I had fixed my first few errors.  There's a very nice flow to setting up flags and conditional dialogue, and I was also able to add layers to that through the stats system. 
### Stats
https://www.youtube.com/watch?v=vEU6MNkttSg
Stats are handled with flags, as detailed in this video. Defaults assume the player gained the "Mystic" ending in Day 2, starting with:

	default rhetoric = 2

	default forgery = 2

	default mysticism = 3

	default reputation = 1
	
These are defined before the start of the game, so can be changed as needed. Starting values will be 1 for day one, and will probably be higher due to choices made in the first two days. 

Gaining stats is through a called label. It looks like this:
		
		label rhetoric_improved:
   	 		$ rhetoric += 1
	   		"Your Rhetoric has improved"
    		return

The player is given opportunities to increase their stats, and some choices require a high enough in a given stat to trigger a flag

### Design and Conditions
So, maybe you'd like a choice guide on how to "Win" the tutorial. Basically, the easiest way to do this is to convince both Amellius and Sempronius to not put on Gladitorial Games. There is more than one way to do this. 

The first thing you'll want to do is to use rhetoric to put the idea in Amellius' head that Sempronius is likely to refuse due to the bad omens. 
The code looks like this: 
	
	label choice1:
    menu:
        "{b}Rhetoric:{/b} Sir, when considering the bad omens, that may not be the case this time. They may not agree.":
            jump choice1_rhetoric

        "I'll go and ask Sempronius":
            jump amellius_exit

	label choice1_rhetoric:
    if rhetoric <= 2:
        a "A fair point, Gaius. Ask them regardless."
        $ amellius_omen = True
        jump amellius_exit
    else:
        show amellius angry
        a "Omens be damned! The people require game to keep their minds off such things! Off you go, Gaius!"
        jump amellius_exit

So, using rhetoric sets the flag amellius_omen. Note that you have to have a rhetoric skill of 2 or higher to do this. This is set by default in the tutorial. 

The dialogue with Lucian presents some influence on the outcome. Honesty is the best policy here, and you should admit to the actor that you think Amellius is wrong. This will set the flag lucian_help. 

Lucian's letter offers some opportunities for improving skills. Immediately agreeing to take the letter will net you +1 reputation, while trying to convince him to deliver it himself will give you +1 rhetoric. These skill ups will be important later. Finally, you can look at the letter, and edit it. Editing it has no downsides at current and gives you +1 to forgery. 

Your first chance to convince Sempronius comes with a Rhetoric or Mysticism check. Rhetoric will need you to have convinced Lucian for that +1 to rhetoric, while Mysticism has a hidden reputation check that needs you to have just taken the letter. Passing these checks will set the sempronius_convinced flag.

If you miss these, there is a chance that Lucian will intervene, as long as the lucian_help flag is set. 

Finally, you will report to Amellius about Sempronius' response. If the amellius_omen flag is triggered, Amellius will give up without a fight. If not, one more Rhetoric and Mysticism check is needed to finally convince Amellius. Mysticism is the "safe" choice, as the requirements are fulfilled by default. If you fail, Amellius will confront Sempronius himself. This will have consequences in Day 4. 

Should neither amellius_omen nor sempronius_convinced be triggered, you will not be able to stop the games. 

Best Ending (Amellius, Sempronius convinced)- Choice1_1, Choice 2_1, Choice 3_2, Choice 4_1, Choice 5_1, Choice 6_1
(Lucian can also intervene to get this ending)

# Wrap-up and Next Steps
Ideally, this Visual Novel demo, as limited as it currently is, should demonstrate some potential of serious Historical Narratives being embedded within Visual Novels, and well as demonstrate some of the storytelling complexity that can be introduced through including a bit of game design and branching story structure. 

Important next steps are to continue iterating on each day, as well as expanding depictions of "Colonial Roman Experience", especially for the downtime sections that were skipped for this demo. This is one thing I would like to have included in this build if not for time restrictions. I would also like to expand my theoretical frameworks, adding pages to the paper. At the moment, it is limited. Ideally it would be about 10000 words. 

Iterating and expanding on the reconstructed environments is in progress, as well as the implementation of more characters. A priority is Servilla, Amellius' wife, who is intended to highlight the liminality of Roman Women, their status as both inferior to men but also holding "soft power" and having lives outside their husband's influence. I would also like to add sounds and music to the game, a feature that had to go entirely overlooked this time. 

Finally, menus and UI will have to be expanded. Creating a map of Pompeii and allowing the player to visit different areas during "downtime" is an idea, as well as a list of Gaius' acquaintances and friends and their current status. Again, borrrowing ideas from Pathologic, as mentioned in the essay.