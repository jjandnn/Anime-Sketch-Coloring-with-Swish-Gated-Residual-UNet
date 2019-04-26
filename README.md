Anime Sketch Coloring with Swish-Gated Residual U-Net
=====================================================

Paper authors: _Gang Liu, Xin Chen, Yanzhong Hu_

Implementation authors: _Alexander Koumis, Amlesh Sivanantham, Pradeep Lam, Georgio Pizzorni_

This is an implementation of the paper _Anime Sketch Coloring with Swish-Gated Residual U-Net_, which uses uses deep learning to colorize anime line-art (sketches).

BIG shout out to the paper authors Xin Chen and Gang Liu for helping us with implementation details that we initially got wrong.

Setup
-----

Use the `requirements.txt` file to install the necessary depedencies for this
project.

```bash
$ pip install -r requirements.txt
```

Training the SGRU Model
-----------------------

Before proceeding, make sure the data folder has the following structure.
```
data/
├── images/
│   ├── images_bw/
│   └── images_rgb/
└── vgg_19.ckpt
```
You can populate the image directories using using your own dataset (see utility script `scripts/process_dir.py` for creating sketch/RGB pairs) or using the original dataset downloadable [here](https://github.com/pradeeplam/Anime-Sketch-Coloring-with-Swish-Gated-Residual-UNet/releases/tag/1.0).
You can find the pretrained `vgg_19.ckpt` checkpoint file [here](http://download.tensorflow.org/models/vgg_19_2016_08_28.tar.gz).
Once you have placed the dataset and VGG checkpoint in your data directory, start the training procedure using the `src/train.py` script:
```bash
$ ./train.py ${DATA_DIR} ${OUTPUT_DIR}
```
Run `./train.py --help` to see the options available for changing hyperparamenters from the command line.

The output directory will have the following structure
```
${OUTPUT_DIR}/
└── ${EXP_NAME:-TIME_STAMP}/
    ├── images/
    ├── events.out.tfevents (tensorboard)
    └── model.ckpt
```

Evaluting pre-trained model
--------------------------
To evaluate a trained model, you must train the weights yourself using the above procedure, or use our pretrained weights available [here](https://github.com/pradeeplam/Anime-Sketch-Coloring-with-Swish-Gated-Residual-UNet/releases/download/1.0/checkpoint.zip). Place your self-trained `model.ckpt.*` files inside of a directory `$CKPT_DIR` or unzip our provided `checkpoint.zip` file into a directory `$CKPT_DIR`.

After you have placed the weights checkpoint files in `$CKPT_DIR`, you can use the script `src/evaluate.py` to see the results on your own image (written as `sketch_image.jpg` below). The best results occur when the input image is 256x256 (similar to the dataset the model was trained on). To display the results, run the script like this:
```bash
./evaluate.py sketch_image.jpg $CKPT_DIR/model.ckpt --show
```
To save the results of into a directory, run the script with the `--output-dir` argument:
```bash
./evaluate.py sketch_image.jpg $CKPT_DIR/model.ckpt --output-dir $OUTPUT_DIR
```
To show and save the results, use both the `--show` and `--output-dir` arguments.

Results
-------
If your model trains correctly it should generate results like this:
![Results](https://i.imgur.com/UDjGVQ8.jpg)
In the above image, the first column is the input sketch image, the second column is the ground truth RGB image, and the rest are generated by the network given the sketch image.
