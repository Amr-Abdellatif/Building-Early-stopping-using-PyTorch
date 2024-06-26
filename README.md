## In this repo we will build early stopping mechanism using pytorch

### Prerequisites

1.  basic knowledge of building models in PyTorch

</div>

<div class="cell markdown">

# Lets create EarlyStopping in pure PyTorch

### What is early stopping ?

A way to stop our model from overfitting

![Alt text](early%20stopping.png)

</div>

<div class="cell code">

``` python
import copy
import torch

class EarlyStopping:
    def __init__(self,patience=5,min_delta = 0,restore_best_weights = True):
        """
        min_delta parameter represents the minimum change in the 
        monitored quantity to qualify as an improvement  
        
        """
        self.patience = patience
        self.min_delta = min_delta
        self.restore_best_weights = restore_best_weights
        self.best_model = None
        self.best_loss = None
        self.counter = 0
        self.status = ""

    def __call__(self,model,val_loss):
        if self.best_loss is None:
            self.best_loss = val_loss
            self.best_model = copy.deepcopy(model.state_dict())
        elif self.best_loss - val_loss >= self.min_delta:
            self.best_model = copy.deepcopy(model.state_dict())
            self.best_loss = val_loss
            self.counter = 0
            self.status = f"Improvement found, counter reset to {self.counter}"

        else:
            self.counter += 1
            self.status = f"No Improvement in the last {self.counter} epochs"
            if self.counter >= self.patience:
                self.status = f"Early Stopping triggered after {self.counter} epochs"
                if self.restore_best_weights:
                    model.load_state_dict(self.best_loss)
                return True
        return False
```

</div>
