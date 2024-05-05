### Exercise 1: CIFAR-10 Using Functional API

**Sequential Strategy**
- The sequential strategy involved stacking convolutional, max-pooling, and dense layers in a linear fashion. The model achieved an 84.02% training accuracy and a 77.95% test accuracy. The training involved gradual reductions in feature map sizes while increasing the depth. Dropout layers were used to prevent overfitting.

**Multiple Paths Strategy**
- This strategy used two distinct paths with different convolutional filter sizes before merging. The intent was to capture features at various scales effectively. It resulted in a slightly higher training accuracy of 86.37% but a lower test accuracy of 75.01% compared to the sequential model. This indicates potential overfitting, where the model may have been too complex relative to the amount of regularization applied.

**Multiple Features Strategy**
- This model utilized three separate paths of convolution and pooling layers, each starting from the input and merging before classification. This strategy achieved a training accuracy of 82.12% and a test accuracy of 67.17%. The lowest test accuracy suggests that while the model might be good at capturing diverse features, it struggles to generalize them effectively.

**Comparison**
- The sequential strategy provided the best balance between training and test performance, indicating good generalization with simpler architecture. The multiple paths and multiple features strategies, despite their higher complexity and capacity to capture a broader range of features, did not perform as well on unseen data, likely due to overfitting. These results underline the importance of proper regularization and perhaps hyperparameter tuning to optimize performance in more complex models.

## Summary of results
The same hyperparameters were used for each training instance:
- epochs: 10
- batch size: 64
- learning rate: 0.001

| Model | Architecture                                                                                                                                                                                                                                                                                                                         | Callback | Acc. train % | Acc. test % |
|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|--------------|-------------|
| 1     | Conv2D(D=32, w=h=3, S=1, P=same) -> Conv2D(D=32, w=h=3, S=1, P=same) -> MaxPooling2D -> Conv2D(D=64, w=h=3, S=1, P=same) -> Conv2D(D=64, w=h=3, S=1, P=same) -> MaxPooling2D -> Conv2D(D=128, w=h=3, S=1, P=same) -> Conv2D(D=128, w=h=3, S=1, P=same) -> MaxPooling2D -> Flatten -> Dropout(0.3) -> Dense(300) -> Dropout(0.3) -> Dense(10) | yes      | 84.02%       | 77.95%      |
| 2     | Path1: Conv2D(D=32, w=h=5, S=1, P=same) -> MaxPooling2D -> Conv2D(D=64, w=h=3, S=1, P=same) -> MaxPooling2D; Path2: Conv2D(D=32, w=h=3, S=1, P=same) x2 -> MaxPooling2D -> Conv2D(D=64, w=h=3, S=1, P=same) x2 -> MaxPooling2D -> Concatenate -> Flatten -> Dropout(0.3) -> Dense(300) -> Dropout(0.3) -> Dense(10)                             | yes      | 86.37%       | 75.01%      |
| 3     | 3 paths each with: Conv2D(D=32, w=h=3, S=1, P=same) -> Dropout(0.2) -> MaxPooling2D, then merged -> Flatten -> Dense(100) -> Dense(10)                                                                                                                                                                                               | yes      | 82.12%       | 67.17%      |

### Exercise 2: Transfer Learning with TensorFlow

**Transfer Learning Approach**
- This approach involves using a pre-trained model (MobileNetV2) as the base, training only the top layers initially, and then fine-tuning the entire model.

**Initial Training (Head Only)**
- Training the head of the network while freezing the pre-trained layers resulted in a quick training process and significant initial learning, with validation accuracy reaching about 67.54%.

**Fine-Tuning**
- Subsequent unfreezing of the entire network for fine-tuning improved the model's ability to tailor the pre-trained features towards the specific dataset, increasing the validation accuracy to approximately 72.04%. This step is crucial as it adapts the deep network features specifically for the task at hand, albeit at the cost of longer training times and more computational resources.

**Testing and Results**
- The final test accuracy after fine-tuning the model was approximately 70.92%, demonstrating effective learning and adaptation to the food images. The process illustrates the power of transfer learning in leveraging pre-existing models trained on large datasets to achieve high performance on more specialized tasks with relatively less data.
