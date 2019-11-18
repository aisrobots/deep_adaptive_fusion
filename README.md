# Choosing Smartly: Adaptive Multimodal Fusion for Object Detection in Changing Environments

This is the code implementing an adaptive gating sensor fusion approach for object detection based on a mixture of convolutional neural networks. More information at our [project page](http://adaptivefusion.cs.uni-freiburg.de)

## Reference
If you find the code helpful please consider citing our work 
```
@INPROCEEDINGS{mees16iros,
  author = {Oier Mees and Andreas Eitel and Wolfram Burgard},
  title = {Choosing Smartly: Adaptive Multimodal Fusion for Object Detection in Changing Environments},
  booktitle = {Proceedings of the International Conference on Intelligent Robots and Systems (IROS)},
  year = 2016,
  address = {Daejeon, South Korea},
  url = {http://ais.informatik.uni-freiburg.de/publications/papers/mees16iros.pdf},
}
```

## Installation
Please refer to [INSTALL.md](INSTALL.md) for setup instructions. The code was tested on Ubuntu 14.04. 
The gating layer implementing the adaptive fusion scheme can be found at [gating_inner_product_layer.cpp](caffe-fast-rcnn/src/caffe/layers/gating_inner_product_layer.cpp) and [gating_inner_product_layer.cu](caffe-fast-rcnn/src/caffe/layers/gating_inner_product_layer.cu)

## Models
We provide several models from the paper. A RGB-D gating network trained on the InOutDoorPeople dataset is available at [googlenet_rgb_depth_gating_iter_2500.caffemodel](models/googlenet_rgb_depth_gating/googlenet_rgb_depth_gating_iter_2500.caffemodel). 
Inference can be made with

```Shell
./tools/test_net.py --gpu 1 --def models/googlenet_rgb_depth_gating/deploy.prototxt \
	--net models/googlenet_rgb_depth_gating/googlenet_rgb_depth_gating_iter_2500.caffemodel \
        --cfg experiments/cfgs/day_night.yml
```

The gating network, i.e. learning how to combine best the convolutional neural networks trained on the RGB and Depth modalities, can be trained with the following command
```Shell
./tools/train_net.py --gpu 1 --solver models/googlenet_rgb_depth_gating/solver.prototxt  \
--weights models/googlenet_rgb_depth_gating/googLeNet_fus.caffemodel --imdb inria_train \
--rand --cfg experiments/cfgs/day_night.yml --iters 10000  2>&1 | tee /tmp/caffe_google_fus.log.$(date +%Y%m%d-%H%M)
```
We merged the two separately trained [RGB](models/googlenet_xss_finetune/googlenet_xss_finetune2_iter_70000.caffemodel) and [Depth](models/depth-google-xxs/googlenet_xss_iter_70000.caffemodel)  expert into a single model [googLeNet_fus.caffemodel](models/googlenet_rgb_depth_gating/googLeNet_fus.caffemodel) in order to train the the adaptive weighting.

## Dataset
Our  InOutDoorPeople dataset containing 8305 annotated frames of RGB and Depth data can be found [here](http://adaptivefusion.cs.uni-freiburg.de/#dataset)

## License
For academic usage, the code is released under the [GPLv3](https://www.gnu.org/licenses/gpl-3.0.en.html) license. For any commercial purpose, please contact the authors.
