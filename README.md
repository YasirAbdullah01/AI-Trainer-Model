# AI-Trainer-Model 
"""
Project: Mini AI Model Trainer Framework
Author: [Your Name]
Description: A professional OOP framework for AI model training pipelines.
"""

from abc import ABC, abstractmethod

class ModelConfig:
    """Stores model settings using Composition."""
    def __init__(self, model_name, learning_rate=0.01, epochs=10):
        # (Concept: Instance Attributes) [span_1](start_span)
        self.model_name = model_name
        self.learning_rate = learning_rate
        self.epochs = epochs

    # (Concept: Magic Method) [span_1](end_span)
    def __repr__(self):
        return f"[Config] {self.model_name} | lr={self.learning_rate} epochs={self.epochs}"

class BaseModel(ABC):
    [span_2](start_span)[span_3](start_span)"""(Concept: Abstraction) - Blueprint for all models[span_2](end_span)[span_3](end_span)"""
    # (Concept: Class Attribute) [span_4](start_span)[span_5](start_span)
    model_count = 0

    def __init__(self, config: ModelConfig):
        # (Concept: Composition) - BaseModel owns a ModelConfig instance[span_4](end_span)[span_5](end_span)
        self.config = config
        BaseModel.model_count += 1

    @abstractmethod
    def train(self, data):
        """Abstract method for training logic."""
        pass

    @abstractmethod
    def evaluate(self, data):
        """Abstract method for evaluation logic."""
        pass

class LinearRegressionModel(BaseModel):
    [span_6](start_span)"""(Concept: Single Inheritance)[span_6](end_span)"""
    def __init__(self, learning_rate=0.01, epochs=10):
        config = ModelConfig("LinearRegression", learning_rate, epochs)
        # (Concept: super()[span_7](start_span)) - Call parent constructor[span_7](end_span)
        super().__init__(config)

    # (Concept: Method Overriding) [span_8](start_span)
    def train(self, data):
        print(f"LinearRegression: Training on {len(data)} samples for {self.config.epochs} epochs (lr={self.config.learning_rate})")

    # (Concept: Instance Method) [span_8](end_span)
    def evaluate(self, data):
        print("LinearRegression: Evaluation MSE=0.042")

class NeuralNetworkModel(BaseModel):
    [span_9](start_span)"""(Concept: Single Inheritance)[span_9](end_span)"""
    def __init__(self, layers, learning_rate=0.001, epochs=20):
        # (Concept: Instance Attribute) [span_10](start_span)[span_11](start_span)
        self.layers = layers
        config = ModelConfig("NeuralNetwork", learning_rate, epochs)
        super().__init__(config)

    # (Concept: Method Overriding) [span_10](end_span)[span_11](end_span)
    def train(self, data):
        print(f"Training on {len(data)} samples for {self.config.epochs} epochs (lr={self.config.learning_rate})")
        print(f"NeuralNetwork layers: {self.layers}")

    def evaluate(self, data):
        print("NeuralNetwork: Evaluation Accuracy=91.5%")

class DataLoader:
    [span_12](start_span)"""(Concept: Independent Class)[span_12](end_span)"""
    def __init__(self, data):
        self.data = data

    def get_data(self):
        """Returns the dataset."""
        return self.data

class Trainer:
    [span_13](start_span)[span_14](start_span)"""(Concept: Aggregation) - Receives DataLoader externally[span_13](end_span)[span_14](end_span)"""
    def __init__(self, model: BaseModel, loader: DataLoader):
        self.model = model
        self.loader = loader

    # (Concept: Polymorphism) - [span_15](start_span)[span_16](start_span)Works with any BaseModel[span_15](end_span)[span_16](end_span)
    def run(self):
        data = self.loader.get_data()
        print(f"---Training {self.model.config.model_name}---")
        self.model.train(data)
        self.model.evaluate(data)

if __name__ == "__main__":
    # [span_17](start_span)Initialize components[span_17](end_span)
    data_loader = DataLoader([1, 2, 3, 4, 5])
    
    # Create Model instances
    lr_model = LinearRegressionModel(learning_rate=0.01, epochs=10)
    nn_model = NeuralNetworkModel(layers=[64, 32, 1], learning_rate=0.001, epochs=20)

    # [span_18](start_span)Display configuration and model count[span_18](end_span)
    print(lr_model.config)
    print(nn_model.config)
    print(f"Models created: {BaseModel.model_count}")

    # Execute training pipeline
    Trainer(lr_model, data_loader).run()
    Trainer(nn_model, data_loader).run()
