# Ros Speech2Text with Ubuntu 16.04 and updated Google Speech Client + use with Team Meeting Jibo Project

This was used with the "Team Meeting Project" using the Jibo robots. All information is current as of 2018-7-16.

A speech2text engine for ROS __(WARNING! This version on this branch uses Ubuntu 16.04 with ROS Kinetic and NOT Indigo)__, using the updated Google Cloud Speech API (Python Client Library v0.27).

For setting up ROS and all that fun stuff look [here](https://alecive.github.io/ros_installation.html).
Just make sure to replace any instance of the word "indigo" with "kinetic" because kinetic is the version of ROS for Ubuntu 16.04
You should also cross reference [this](http://wiki.ros.org/kinetic/Installation/Ubuntu) as you go just to make sure anything kinetic specific is executed properly.

Once you have made sure that you have built and sourced your new catkin_ws, make sure to make your scripts executable. In terminal, go to the location of your python scripts. For this specific project, these are located in `scripts` folders. Once there, run `chmod +x [name of file]` and open a new terminal and you should be able to run using roslaunch.

You may also run into some problems with `pip` if you are starting from scratch (i.e. fresh installation of Ubuntu 16.04). One error I got a bunch was `AttributeError: 'module' object has no attribute 'SSL_ST_INIT'`. To fix this, just run `sudo python -m easy_install --upgrade pyOpenSSL` and it should be fine.

For most all other warnings and errors, you should be able to just do something like `sudo pip install [name] --upgrade`.

Just make sure that before trying to do `pip install pyaudio` you run this line first `sudo apt-get install libasound-dev portaudio19-dev libportaudio2 libportaudiocpp0`

__NOTE:__ "SAMSON STAGE PXD1" microphones were used in our project, and if you want to use them again, open up the back and use the screwdriver to set the gain to in between the fourth and fifth tick from the bottom. This sets the sensitivity to a place that easily detects the wearer's voice but not other sounds/voices. But test this yourself because depending on the surroundings, you may need higher or lower sensitivity. Also, make sure to have the mics directly facing your mouth and not to the side of your mouth, or you may get unpleasant results.

When creating individual nodes for the SAMSON mics, use the option to set the mics based on their names and not their numerical id. The numerical id often changes and sometimes just straight up doesn't work. So using the name "hw:__#__,0" works much better. Just please don't assume the hardware numbers for the mics start at 1. This is not always the case, especially if you have a dedicated graphics card or something else like that. If in the case that you run the launch file and keep getting an error something along the lines of unable to find the mic, then just close all your open terminals and restart roscore and relaunch the files. You shouldn't need to do this, but also this should always work (given that all your code is correct)

# Running in terminal:
In terminal, make sure you run `roscore` first before trying to run the other files

Once you have `roscore` up and running, open another terminal window or tab and run `roslaunch team_meeting_project send_speech_to_tablet.launch` or `roslaunch team_meeting_project send_speech_to_local.launch` whether you want to run the code with a TCP connection to a tablet or locally without the need for the TCP connection

In another terminal window or tab run `roslaunch ros_speech2text ros_speech2text.launch` to run with only one mic or `roslaunch ros_speech2text ros_speech2text_[2, 3, or 4]mics.launch` depending on how many mics you want to run with. For example if I want to run with 3 mics, I would run `roslaunch ros_speech2text ros_speech2text_3mics.launch`

__Note:__
In the case that the mics become, for whatever reason, out of order (i.e. mic 1 is no longer associated with pid 1), then unplug all of the mics, and run the different launch files in order of increasing number of mics while adding the appropriate mics one at a time.
For example, with all of mics unplugged, insert the receiver for mic 1 and then run `roslaunch ros_speech2text ros_speech2text.launch` then add the second mic receiver and run the launch file for 2 mics and so on. You may need to exit out of all open terminals and restart terminals.

If you want to control whether or not to use the start_utterance messages, look in the `ros_speech2text` launch files and find the parameter `enable_start_utterance`.

# Using the updated Google-cloud speech-to-text API
Take a look at these pages (navigating Google's documentation can be kind of annoying sometimes):

[first place to look](https://cloud.google.com/speech-to-text/docs/basics)

[second place to look](https://google-cloud-python.readthedocs.io/en/latest/speech/index.html)

[migration from old Google API](https://cloud.google.com/speech-to-text/docs/python-client-migration)

[stable version of speech client](https://google-cloud-python.readthedocs.io/en/latest/speech/gapic/v1/api.html)

[documentation on methods](https://cloud.google.com/speech-to-text/docs/reference/rpc/google.cloud.speech.v1)

[beta version of client with added functionality like auto punctuation](https://google-cloud-python.readthedocs.io/en/latest/speech/gapic/v1p1beta1/api.html)

[auto punctuation documentation](https://cloud.google.com/speech-to-text/docs/automatic-punctuation)
(I don't really notice a difference in terms of recognition speed, so it could be cool to keep testing this out)

For information on how to analyze the transcript for things like getting the sentiment of the sentence or grabbing the nouns and verbs of the sentence, look here:

[first place to look](https://cloud.google.com/natural-language/docs/basics)

[second place to look](https://google-cloud-python.readthedocs.io/en/latest/language/usage.html)

[for analyzing syntax](https://cloud.google.com/natural-language/docs/analyzing-syntax)
