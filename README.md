# Download and pre-process ROAD dataset

Here, we release the download and pre-processing instructions for ROAD dataset. It is released with a [paper](https://arxiv.org/pdf/2102.11585.pdf) and [3D-RetinaNet](https://github.com/gurkirt/3D-ReintaNet) code as a baseline. Which also contains evaluation code. ROAD dataset will be used with [The ROAD challenge](https://sites.google.com/view/roadchallangeiccv2021/).


## Main Features

- Action annotations for human as well as other road agents, e.g. Turning-right, Moving-away etc. 
- Agent type labels, e.g. Pedestrian, Car, Cyclist, Large-Vehicle, Emergency-Vehicle etc.
- Semantic location labels of the location of agent, e.g. in vehicle lane, in right pavement etc.
- 122K frames from 22 videos annotated, each video is 8 mins long on an average.
- track/tube id annotated for every bounding box on every frame for every agent in the scene.
- 7K tubes/tracks of individual agents.
    - Each tube consists  on  average  of  approximately  80  bounding  boxes linked  over  time.
- 559K bounding  box-level agent  labels.
- 641K and 498K bounding box-level action and location labels.
- 122k annotated with self/ego-actions of AV as well, e.g. AV-on-the-mov, AV-Stopped, AV-turning-right, AV-Overtaking etc.

## Attribution
ROAD dataset is build upon [Oxford Robot Car Dataset (OxRD)](https://robotcar-dataset.robots.ox.ac.uk/about/). Please cite the original dataset if it useful in your work, citation can be found [here](https://robotcar-dataset.robots.ox.ac.uk/citation/). 

Similar to original work [(OxRD)](https://robotcar-dataset.robots.ox.ac.uk/privacy/), this work is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0) International License and is intended for non-commercial academic use. If you are interested in using the dataset for commercial purposes please contact original creator [OxRD](https://robotcar-dataset.robots.ox.ac.uk/contact/) for video content and [Fabio](https://cms.brookes.ac.uk/staff/FabioCuzzolin/) and [Gurkirt](http://gurkirt.github.io/) for event annotations.

If you use ROAD dataset please cite:

    @misc{singh2021road,
          title={ROAD: The ROad event Awareness Dataset for Autonomous Driving}, 
          author={Gurkirt Singh and Stephen Akrigg and Manuele Di Maio and Valentina Fontana and Reza Javanmard Alitappeh and Suman Saha and Kossar Jeddisaravi and Farzad Yousefi and Jacob Culley and Tom Nicholson and Jordan Omokeowa and Salman Khan and Stanislao Grazioso and Andrew Bradley and Giuseppe Di Gironimo and Fabio Cuzzolin},
          year={2021},
          eprint={2102.11585},
          archivePrefix={arXiv},
          primaryClass={cs.CV}
    }
        

## Download

BY DOWNLOADING THE DATASET VIDEOS YOU ARE BOUNDED TO ADHERE TO PRIVACY GUIDELINES OF [OxRD](https://robotcar-dataset.robots.ox.ac.uk/privacy/). PLEASE VISIT [OxRD](https://robotcar-dataset.robots.ox.ac.uk/privacy/) PRIVACY POLICY FOR MORE DETAILS. VIDEOS FROM OxRD AND PROVIDED ANNOTATIONS ARE ONLY FOR ACADEMIC PURPOSE. 

We release annotations annotated by [Visual Artificial Intelligence Laboratory](https://cms.brookes.ac.uk/staff/FabioCuzzolin/) and pre-processed videos from [OxRD](https://robotcar-dataset.robots.ox.ac.uk/about/). Pre-processing includes `demosaic` for RGB conversion, `ffmpeg` for `.mp4` conversion and fixing the frame-rate. More details can be found in [tar2mp4](./tar2mp4/README.md).

You can download the `Train-Val-set` videos and annotation by changing your current directory to the road directory and running the bash file [get_dataset.sh](./road/get_dataset.sh) will automatically download the annotation files and video directory in the currect directory (road).
```
bash get_dataset.sh
```
OR 
You can download the `Train-Val-set` videos and annotation from [Google-Drive link](https://drive.google.com/drive/folders/1hCLlgRqsJBONHgwGPvVu8VWXxlyYKCq-?usp=sharing)

Private video of `Test-set` will be released in accordance with the [The ROAD challenge](https://sites.google.com/view/roadchallangeiccv2021/).

## Frame-extraction

Baseline code for [3D-RetinaNet](https://github.com/gurkirt/3D-ReintaNet) used in dataset release [paper](https://arxiv.org/pdf/2102.11585.pdf) uses sequences of frames as input. Once you have downloaded the videos from Google-Drive, create a folder name `road` and put annotation under it, create another folder name `videos` under `road` folder, put all the videos under folder name `videos`. Now, your folder structure looks like:

```
    road/
        - road_trainval_v1.0.json
        - videos/
            - 2014-06-25-16-45-34_stereo_centre_02
            - 2014-06-26-09-53-12_stereo_centre_02
            - ........

```

Now, you can use `extract_videos2jpgs.py` to extract frames. You will need to provide path to `road` folder as an argument. You will need `ffmpeg` installed on your machine or your python should include its binaries, `sudo apt install ffmpeg` should do it on Ubuntu.

```
python extract_videos2jpgs.py <path-to-road-folder>/road/
```

Now, the `road` directory would look like.

```
    road/
        - road_trainval_v1.0.json
        - videos/
            - 2014-06-25-16-45-34_stereo_centre_02
            - 2014-06-26-09-53-12_stereo_centre_02
            - ........
        - rgb-images
            - 2014-06-25-16-45-34_stereo_centre_02/
                - 00001.jpg
                - 00002.jpg
                - .........*.jpg
            - 2014-06-26-09-53-12_stereo_centre_02
                - 00001.jpg
                - 00002.jpg
                - .........*.jpg
            - ......../
                - ........*.jpg

```
## Annotation Structure

Annotation for train validation split are saved in single `json` file named `road_trainval_v1.0.json`. It is located under root directory of the dataset as can be seen above.

The first level of `road_trainval_v1.0.json` contain dataset level information like classes of each label type.

```

dict_keys(['all_input_labels', 'all_av_action_labels', 'av_action_labels', 'agent_labels', 'action_labels', 'duplex_labels', 'triplet_labels', 'loc_labels', 'db', 'label_types', 'all_duplex_labels', 'all_triplet_labels', 'all_agent_labels', 'all_loc_labels', 'all_action_labels', 'duplex_childs', 'triplet_childs'])

``` 
- `all_input_labels`: All classes used to annotate the dataset
- `label_types` :  It is list of all the label types `['agent', 'action', 'loc', 'duplex', 'triplet']`.
- `all_av_action_labels`: All classes used to annotate AV actions
- `av_action_labels`: Classes finally being used for AV actions
-  Remaining fields ending with `labels` follows the same logic and AV actions described in above line.
- `duplex_childs` and `triplet_childs` contain ids of child classes form `agent`, `action` or `location` labels to construct `duplex` or `triplet` labels, 
- `duplex` is constructed using `agent` and `action` classes.
- `event` or `triplet` is constructed  using `agent`, `action`, and `location` classes.

Finally, `db` field contain all `frame` and `tube` level annotation for all the videos. 

- To access annotation for a vides, use db['2014-06-25-16-45-34_stereo_centre_02'], where `'2014-06-25-16-45-34_stereo_centre_02'` is name of a video.
- Each video annotation comes with following fields
    - `['split_ids', 'agent_tubes', 'action_tubes', 'loc_tubes', 'duplex_tubes', 'triplet_tubes', 'av_action_tubes', 'frame_labels', 'frames', 'numf']`
    - `split_ids` contains the split id assigned this videos out of `'test', 'train_1','val_1',........'val_3'`. 

## Evaluation

After this you are ready to train or test [3D-RetinaNet](https://github.com/gurkirt/3D-ReintaNet). Which contain dataloader class and evaluation scripts for all the tasks in ROAD dataset. 
