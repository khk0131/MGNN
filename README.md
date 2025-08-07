# mgnn
Hello! MGNN is used for neural network potential based on equivariant graph neural network E(3)-equivariant neural network consisting atom-centered message passing neural networks (MPNNs) can be used for molecular dynamics simulations.<br><br>The model is originated from NequIP(https://www.nature.com/articles/s41467-023-36329-y) which employs E(3)-equivariant convolutions for interactions of geometric tensors, resulting in a more information-rich and faithful representation of atomic environments.

## Installation
mgnn requires e3nn puckage(https://github.com/e3nn/e3nn)<br>
```bash
git clone git@github.com:khk0131/MGNN.git
```
Then, you can install mgnn in your environment

## Set Path
```bash
# set the following paths in .bashrc
export PYTHONPATH="${PYTHONPATH}:$HOME/mgnn"
export PATH="${PATH}:$HOME/mgnn/mgnn/scripts"
```
## Training
Firts of all, config.yaml file should be creadted and follwing information should be included

```bash
# list with dataset paths
train_dataset_paths: [
/nfshome18/khosono/vasp/train_dataset/H2O_density1.0, # AIMD dataset with pickle package 
]

shuffle_dataset: True # Shuffle dataset
dataloader_num_workers: 0

# training
device: cuda
epoch: 100
frame_skip_num: 5  # How many steps will be skipped for learning AIMD data
loss_pot_ratio_step_size: 200000 # loss_pot_ratio is multiplied with loss_pot_ratio_gamma at each this step
loss_pot_ratio_initial: 1.0 # initial loss_pot_ratio value
loss_pot_ratio_gamma: 1.0

loss_force_ratio_step_size: 200000
loss_force_ratio_initial: 1.0
loss_force_ratio_gamma: 1.0

auto_resume: True # Training resume in the middle or not

# optimizer
lr: 0.001 # learning rate
adam_betas: [0.9, 0.999] # Hypermarameter for Adam
adam_weight_decay: 0.0 # Hyperparameter for Adam

# scheduler
steplr_step_size: 200000 # lr is mutiplied with steplr_gamma at each this step
steplr_gamma: 0.1
min_lr: 1e-06

# output
save_model_step_size: 1000 # save models at each this step
state_dict_dir: state_dicts # state_dict is saved in the directory
save_frozen_model_dir: frozen_models # frozen_model is saved in the directory

# mgnn
model: Nequip
num_atom_types: 5
cut_off: 3.4
lmax: 1
parity: 1
num_layers: 1
bessel_num_basis: 8
bessel_basis_trainable: False
polynominal_p: 8.0
edge_sh_normalization: component
edge_sh_normalize: True
env_embed_multiplicity: 32
latents_to_edge_energy_hidden_dims: [128]
two_body_mlp_hidden_dims: [128]
env_embed_mlp_hidden_dims: []
latent_mlp_hidden_dims: [128]
activation_function: silu
```
