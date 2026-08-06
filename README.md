|   Model           | Accuracy |
| ----------------- |-------- |
| ```resnet18``` - baseline   | 78.89% |
| ```resnet18-paraboloidout```         | **79.56%** |



|   Model           | Momentum | Accuracy |
| ----------------- |-------- | -------- |
| ```resnet18``` - baseline   | 0.9, nesterov = True | 78.89% |
| ```resnet18-paraboloidout```   |   0.1, nesterov = False   | 78.96% |
| ```resnet18-paraboloidout```   |   0.2, nesterov = False   | 78.82% |
| ```resnet18-paraboloidout```   |   0.4, nesterov = False   | 79.12% |
| ```resnet18-paraboloidout```   |   0.5, nesterov = False   | 79.14% |
| ```resnet18-paraboloidout```   |   0.6, nesterov = False   | 79.37% |
| ```resnet18-paraboloidout```   |   0.7, nesterov = False   | **79.56%** |
| ```resnet18-paraboloidout```   |   0.8, nesterov = False   | 78.87% |
| ```resnet18-paraboloidout```   |   0.9, nesterov = False   | 77.46% |
