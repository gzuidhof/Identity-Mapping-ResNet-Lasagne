# Identity Mappings in Deep Residual Networks in Lasagne/Theano

Reproduction of some of the results from the recent [MSRA ResNet](https://arxiv.org/abs/1603.05027) paper. Exploring the full-preactivation style residual layers.

![PreResNet](https://qiita-image-store.s3.amazonaws.com/0/100523/a156a5c2-026b-de55-a6fb-e4fa1772b42c.png)

## Results

Results are presented as classification error percent.

| ResNet Type | Original Paper | My Results |
| -----------|-----------|----------- |
| ResNet-110 | 6.37 | 6.38 |
| ResNet-164 | 5.46 | 5.66 |

**Note:** ResNet-110 is the stacked 3x3 filter variant and ResNet-164 is the 'botttleneck' architecture. Both use the new pre-activation units as proposed in the paper.

### ResNet-110

![ResNet-110](http://i.imgur.com/Y7VrxOC.png)

### ResNet-164

![ResNet-164](http://i.imgur.com/VznjI5x.png)

## Implementation details

Had to use batch sizes of 64 for ResNet-110 and 48 for ResNet-164 due to hardware constraints. The data augmentation is exactly the same, only translations by padding then copping and left-right flipping.

## Pre-Trained weights

The weights of the trained networks are available for download.

## Running the networks

To run your own PreResNet simply call train.py with system args defining the type and depth of the network.

```
train.py [type] [depth] [width]
```

-**Type (string)**:  Can be 'normal', 'bottleneck' or 'wide'

-**Depth (integer)**:  Serves as the multiplier for how many residual blocks to insert into each section of the network

-**Width (integer)**: Only for wide-ResNet, servers as the filter multiplier [3x3, 16*k] for residual blocks, excluding the first convolution layer.

| Group | Size | Multiplier |
| ------|:------:|:----------:|
| Conv1 | [3x3, 16] | - |
| Conv2 | [3x3, 16]<br>[3x3, 16] | N |
| Conv3 | [3x3, 32]<br>[3x3, 32] | N |
| Conv4 | [3x3, 64]<br>[3x3, 64] | N |
| Avg-Pool | 8x8 | - |
| Softmax  | 10 | - |

**Note:** If using the wide-ResNet, the implementation in the [paper](https://arxiv.org/pdf/1605.07146v1.pdf) will be slightly different than the one here. They use different preprocessing and a different value for L2. This repo stays consistent with the [MSRA paper](https://arxiv.org/abs/1603.05027).

### References

* Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun, "Identity Mappings in Deep Residual Networks", [link](https://arxiv.org/pdf/1603.05027v2.pdf)
* Sergey Zagoruyko, Nikos Komodakis, "Wide Residual Networks", [link](https://arxiv.org/pdf/1605.07146v1.pdf)
