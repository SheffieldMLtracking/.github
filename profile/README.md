# Sheffield Machine Learning Tracking

[Mike's](https://www.sheffield.ac.uk/dcs/people/academic/michael-smith) website: [michaeltsmith.org.uk](http://michaeltsmith.org.uk/)

# Contributing

Please read the [contribution guide](../CONTRIBUTING.md).

Also see the [BBSRC Project: Pesticide Induced Changes to Pollinator Behaviour](https://github.com/orgs/SheffieldMLtracking/projects/1/) board on GitHub Projects.

# File Structures

The images from [bee_track](https://github.com/lionfish0/bee_track) are stored in this file structure:

`<session>/<set>/<device id>/<camera id>/<timestamp>_<photo id>.np`

- the 'session' is what calibration will work on -- it will assume all the photos from within the same session have the same alignment. If a camera gets moved, a new session folder will be needed.
- the 'set' is for the convenience of the research -- it could be e.g. "learningflight12" or "tomato23". A special set called 'cal' is for the use of the calibration tool, and contains photos of the calibration QR code for 3d camera registration.
- the 'device id' is a unique label for each of the "boxes".
- the 'camera id' is the id of the camera inside the box (there are sometimes two or more internal cameras).
- the 'timestamp' is a YYYMMDD_HHMMSS.UUUUUU string of when the photo was taken in the UTC time zone
- the 'photo_id' is an incrementing label (000000,000001,000002,...)

# Typical 3d Flight Path Inference Pipeline

1. Copy all the separate camera systems to a single location (this should result in the file structure above). For this example we'll assume they are:
<pre>
  ~/beephotos
      20240510
          session_20240510_092314  <-- session names could default to the datetime
              learningflight1 <-- the set
                  box1 <-- the id of the camera box
                      AF54234ASF <-- the id of one of the internal cameras
                          20240510_092314.123412_000001.np
                          20240510_092314.643423_000002.np
                            :               :       :
                      HRF354234D <-- the id of another of the internal cameras
                          20240510_092314.123412_000001.np
                          20240510_092314.643423_000002.np
                            :               :       :
                  box2 <-- the id of the camera box
                      ZZ2342341S <-- the id of one of the internal cameras
                          20240510_092314.312341_000012.np
                          20240510_092314.923141_000013.np
                            :               :       :
                      AA9345ASSD <-- the id of another of the internal cameras
                          20240510_092314.312341_000012.np <--- @Joe I assume the photoid is unique to the trigger (but the photos taken by all the internal cameras at that same moment will have the same id). Do we mind that the only distinction between the files in this folder and the other are their location (the filename doesn't contain enough info).
                          20240510_092314.923141_000013.np                          
                            :               :       :
                        etc...
              cal <-- the set (this is a specially named set, that means it contains calibration photos -- with the QR code in).
                  box1 <-- the id of the camera box
                      AF54234ASF <-- the id of one of the internal cameras
                          20240510_092501.435232_000015.np
                          20240510_092503.103022_000016.np
                            :               :       :
                      HRF354234D <-- the id of another of the internal cameras
                          20240510_092501.435232_000015.np
                          20240510_092503.103022_000016.np
                            :               :       :
                  box2 <-- the id of the camera box
                      ZZ2342341S <-- the id of one of the internal cameras
                          20240510_092501.510223_000015.np
                          20240510_092503_202301_000016.np
                            :               :       :
                      AA9345ASSD <-- the id of another of the internal cameras
                          20240510_092501.510223_000015.np
                          20240510_092503_202301_000016.np                       
                            :               :       :                        
</pre>
2. Use the automated tag detection to find the tags in the photos (this can take a while for lots of photos, so you might want to run it on a subset). e.g.
`btretrodetect ~/beephotos/20240510 --from 10:30 --to 14:30` @Joe/Chris - it might make sense that this command line tool can be given any step in the directory structure:
`btretrodetect ~/beephotos/20240510/session_20240510_092314/learningflight1`
or could it even take wildcards? `btretrodetect ~/beephotos/20240510/session_20240510_092314/*/box1`
This will update or create a .json file next to each .np image file (same name but for the .json).

3. A human needs to manually click on the tags to confirm them:
`btviewer ~/beephotos/20240510/session_20240510_092314/learningflight1`
This will either update or create a .json file next to each .np image file.

4. Calibration will be needed:
`btcalibrate ~/beephotos/20240510/session_20240510_092314` will run the calibration on just this session (it will look for a 'cal' folder). The results will be put into a .json file in the session_20240510_092314 root folder. The commandline tool will also report diagnoistics.
One could call `btviewer ~/beephotos/20240510/session_20240510_092314/cal` to view the results of the calibration (check that it has been done correctly).

5. Once the bees have been manually labelled, and the calibration has been done, we can recompute the 3d flight path.
`btinferpath ~/beephotos/20240510/session_20240510_092314/learningflight1 --from 09:23:14 --to 09:23:17`
this will create a json file:
`infer.json`
if there is a time range then this will be included in the filename:
`infer_092314_092317.json`
One option (on by default) is to include in the json the x/y coordinates projected onto each camera.

6. Finally, you might want to see the results using btviewer:
`btviewer ~/beephotos/20240510/session_20240510_092314/learningflight1`
this will look for an infer.json or infer_<starttime>_<endtime>.json and pull out the projected coordinates from that for the camera.

# Contents of json metadata files

Each image may have an associated set of points (generated from btretrodetect, btalignment, the btviewer or btinference).
Each tool adds a directory to the folder containing the images, for example, in `~/beephotos/2024-05-10/session2/learningflight1/box3/ABC1232141/retrodectect'
Then inside this folder, are json files, up to one for each image in the images-folder. Each one is the name of the image but with .np replaced with .json
For example: `20240613_10+29+45.869012_0014.json`.

So the viewer needs to check what directories exist, and then look for all the json files with the same name.

Not sure if this [example](https://github.com/SheffieldMLtracking/.github/blob/main/examples/tags_btviewer_uniqueid.json) is currently correct.
