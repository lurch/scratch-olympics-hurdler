# The Scratch Olympics Hurdler

In this activity you will make a hurdles game using Scratch, where the speed of the runner is controlled by how fast you can hit the `x` and `z` keys, and perfect timing is required to jump over the hurdles at exactly the right time.

## Getting hold of the sprites
- Download this [zip file](images/assets.zip) to your home directory and unzip it's contents.

## Setting up the assets.
![ui](images/ui.gif)
1. Open Scratch by clicking on `Menu` > `Programming` > `Scratch`.
1. Now, click on the `background` icon and import the new background from the ScratchOlympicsAssets directory. You can then delete the old background.
1. Click on the icon to import a new sprite and then choose the `run-1` image. Then import `run-2`, `run-3` and `run-4` as additional costumes. You can then delete the old cat sprite.

## Capturing the key mashing.
1. The first step is to capture the `x` and `z` key presses, and use the speed at which the player is pushing the keys, to control the size of a variable
1. To do this you'll need a variable that stores the last known key press. Create a variable called `last_key` and set it to `z` when the green flag is clicked.

	![script](images/greenflag1.png)

1. For the next script you will need a new variable called `speed`, so go ahead and create it now. It can be set to `0` when the game begins.

	``` scratch
	when green flag clicked
	set [last_key v] to [z]
	set [speed v] to [0]
	```

	![script](images/greenflag2.png)

1. When the `x` key is pressed, if the `last_key` is equal to `z`, then the `speed` variable can be increased and the `last_key` can be set to `x`. This will ensure that the player can't cheat and keep hitting the `x` key to make the speed increase.

	![script](images/x_script.png)

1. The same can be done for the `z` key. In combination, these two scripts force the player to hit the keys _alternately_ in order for the speed variable to be increased.

	![script](images/z_script.png)

1. Now test your script. Click the green flag and then repeatedly press the `x` and `z` keys and watch the speed variable increase.

## Animating the hurdler.

1. At the moment, the hurdler has 4 costumes as part of what is called a _walk cycle_ (or run cycle in this case). When these costumes are switched the character appears to run on the spot. The time delay between costume switches should depend on the `speed` variable. The higher the `speed` the quicker the costume change should be and therefore the smaller the delay. You can get this effect by _dividing_ `1` by the `speed` variable to calculate a delay.

	``` scratch
	when green flag clicked
	forever
	switch costume to [run-1 v]
	wait ([1]/(speed)) secs
	switch costume to [run-2 v]
	wait ([1]/(speed)) secs
	switch costume to [run-3 v]
	wait ([1]/(speed)) secs
	switch costume to [run-4 v]
	wait ([1]/(speed)) secs
	```
	![script](images/run1.png)

1. If you run this script as it is, you will get an error though. This is because `speed` starts off with a value of `0`. So the computer is trying to calculate `1 / 0` which it can't do. This is a very common error that programmers make in their code. To fix this you can use a conditional to make sure that the calculation only occurs when `speed` is larger that `0`

	``` scratch
	when green flag clicked
	forever
	if <(speed) > [0]>
	switch to costume [run-1 v]
	wait ([1]/(speed)) secs
	switch to costume [run-2 v]
	wait ([1]/(speed)) secs
	switch to costume [run-3 v]
	wait ([1]/(speed)) secs
	switch to costume [run-4 v]
	wait ([1]/(speed)) secs
	```

	![script](images/run2.png)

1. Now you should be able to test your script and watch the hurdler running on the spot as you press the `x` and `z` keys.

## Jumping

1. Hurdler's need to jump. You'll need a couple more costumes for this part, so look in the runner 
directory and import the jump-1 and jump-2 costumes for your hurdler.

1. You'll need a new variable for this part called `jumping`. This is because other scripts will need to know when the character is jumping. Create the new variable and set it to `False`

	``` scratch
	when green flag clicked
	set [last_key v] to [z]
	set [speed v] to [0]
	set [jumping v] to [False]
	```
	
	![script](images/greenflag3.png)
	
1. The character should jump when the space bar is pressed. The first thing that happens is the `jumping` variable should be set to `True`. Then the costume can be changed to `jump-1` and the character can glide upwards. Then the costume can be changed to `jump-2` and the character can glide back down again. Finally the `jumping` variable can be returned to `False` to indicate that the jumping animation has finished.

	``` scratch
	when [space v]key pressed
	set [jumping v] to [True]
	switch to costume [jump-1 v]
	glide [0.5] secs to x: [-104] y [32]
	switch to costume [jump-2 v]
	glide [0.5] secs to x: [-104] y [-32]
	set [jumping v] to [False]
	```

	![script](images/jump.png)
	
1. Test your script and it might surprise you to see that the character's costume doesn't change. This is because the _walk cycle_ you set up previously is still working. You'll need to stop this _walk cycle_ when the character is jumping. To do this, you can use an `and` conditional operator to check that both `speed > 0 and jumping = False` for the _walk cycle_ to work.

	``` scratch
	when green flag clicked
	forever
	if <<(speed) > [0]>and<(jumping) = [False]>>
	switch to costume [run-1 v]
	wait ([1]/(speed)) secs
	switch to costume [run-2 v]
	wait ([1]/(speed)) secs
	switch to costume [run-3 v]
	wait ([1]/(speed)) secs
	switch to costume [run-4 v]
	wait ([1]/(speed)) secs
	```
	
	![script](images/run3.png)
	
1. Now have a go and you should find your character jumps when the space key is pressed.

## Slowing Down
1. At the moment the more you press the `x` and `z` keys, that faster and faster the character runs. There needs to be a way of slowing the hurdler down, so she doesn't win too easily. This can be done on your initial script that sets the starting variables. You just need to add an infinite loop that will check if the speed is greater than 1 and then lower it every few 100ths of a second.


	``` scratch
	when green flag clicked
	set [last_key v] to [z]
	set [speed v] to [0]
	set [jumping v] to [False]
	forever
	if <(speed) > [1]>
	change [speed v] by [1]
	wait [0.5] secs
	```

	![script](images/greenflag4.png)

## Adding in hurdles

For the final part of this worksheet, you can add in hurdles that the character will have to jump over.

1. Import the hurdle.png sprite from the ScratchOlympicsAssets/items directory.
1. This sprite needs to begin at the far right of the screen. Then it should continually move left across the screen at a pace that is proportinal to the speed of the character. When it hits the far left of the screen it should instantly appear on the right again.

	``` scratch
	when green flag clicked
	go to x: [230] y:[-77]
	forever
	if <(speed) > [1]>
	change x by [-10]
	wait <[1]/(speed)> secs
	if <(x position) < [-230]>
	go to x:[230] y:[-77]
	```
	
	![script](images/hurdles1.png)

## Making the hurdles an obstacle

At the moment the runner can just plough straight through the hurdles. She needs to be slowed down if she doesn't jump.

1. Back on the hurdler sprite, add in a new `when green flag clicked` block.
1. This next part is a little complicated. The runner should be slowed down if she:
  1. Isn't jumping
  1. Has an x position just before the hurdle
  1. Has an x position just after the hurdle
1. This can be acieved using two `and` logical operators - checking if:
  1. `jumping = False`
  1. `x position > x position of hurdle - 5`
  1. `x position < x position of hurdle + 5`
1. If all those conditions are met, then she must have hit the hurdle and her speed can be dropped.
