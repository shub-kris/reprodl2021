# Reproducible Deep Learning
## Exercise 2: Configuration with Hydra
[[Official website](https://www.sscardapane.it/teaching/reproducibledl/)]

## Objectives for the exercise

- [x] Adding Hydra support for configuration.

1. Install [Hydra](https://github.com/facebookresearch/hydra):

```bash
pip install hydra-core
```

## Instructions

The aim of this exercise is to move all configuration for the training script inside an external configuration file. This simple step dramatically simplifies development and reproducibility, by providing a single entry point for most hyper-parameters of the model.

1. Go through the training script, and make a list of all values that can be considered hyper-parameters (e.g., the **learning rate** of the optimizer, the **sampling rate** of the dataset, the **batch size**, ...).

2. Prepare a `configs/default.yaml` file collecting all hyper-parameters. We suggest the following (macro) organization, but you are free to experiment:

```yaml
data:
    # All parameters related to the dataset
model:
    # All parameters related to the model
    optimizer:
        # Subset of parameters related to the optimizer
trainer:
    # All parameters to be passed at the Trainer object
```


3. Decorate the `train()` function to accept the configuration file:

```python
@hydra.main(config_path='configs', config_name='default')
def train(cfg: DictConfig):
    # ...
```

4. Use the `cfg` object to set all hyper-parameters in the script.
   * Read the [OmegaConf](https://omegaconf.readthedocs.io/en/2.0_branch/usage.html#access-and-manipulation) documentation to learn more about accessing the values of the `DictConfig` object.
   * `LightningModule` [accepts dictionaries to set hyper-parameters](https://pytorch-lightning.readthedocs.io/en/latest/common/hyperparameters.html).
   * The `Trainer` object requires key-value parameters, but you can easily solve this with:
        ```python
        trainer.fit(**cfg.trainer)
        ```
    * Hydra will run your script inside a folder which is dynamically created at runtime. To load the dataset, you can use `hydra.utils.get_original_cwd()` to [recover the original folder](https://hydra.cc/docs/tutorials/basic/running_your_app/working_directory#original-working-directory).

If everything went well, you should be able to experiment a little bit with Hydra configuration management:

- Run a standard training loop: 
  
  ``` python train.py ```
- Dynamically change the batch size (modify according to your YAML file): 
  
  ``` python train.py data.batch_size=4 ```

- Add new flags for the trainer on-the-fly:
  
  ``` python train.py +trainer.fast_dev_run=True ```

- Remove a flag from the configuration:
   
  ``` python train.py ~trainer.gpus ```
  
- Change the output directory:
   
  ``` python train.py hydra.run.dir="custom_dir" ```


### Add some logging

Using Hydra, we can easily add any amount of logging into the application, that is automatically saved inside the dynamically generated folders.

Start by importing the logging function:
  
```python 
import logging
logger = logging.getLogger(__name__)
```

You can use the logger to save some important information, debug messages, errors, etc. You will find the log inside the `train.log` file in each generated folder. For example, you can log the initial configuration of the training script:

```python 
from omegaconf import OmegaConf
logger.info(OmegaConf.to_yaml(cfg))
```