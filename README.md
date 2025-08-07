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

# Full configs
```bash
# dataset and dataloader
train_dataset_paths: [
/nfshome15/rkudo/vasp/train_dataset/H2O_density1.0,
]

seed: 0 # 初期重みを決定するシード
shuffle_dataset: True # データセットの順番をシャッフルするかどうか
dataloader_num_workers: 0

# training
device: cuda
epoch: 100
frame_skip_num: 5  # １本のMDをいくつおきに訓練するか
loss_pot_ratio_step_size: 200000 # このステップごとに、loss_pot_ratioをloss_pot_ratio_gamma倍にする
loss_pot_ratio_initial: 1.0 # 初期のloss_pot_ratioの値
loss_pot_ratio_gamma: 1.0

loss_force_ratio_step_size: 200000
loss_force_ratio_initial: 1.0
loss_force_ratio_gamma: 1.0

flag_calc_stress: False # Trueならばvirialを学習させる
loss_stress_ratio_step_size: 200000 # このステップごとに、loss_virial_ratioをloss_virial_ratio_gamma倍にする
loss_stress_ratio_initial: 1.0 # 初期のloss_virial_ratioの値
loss_stress_ratio_gamma: 1.0

auto_resume: True # 自動で途中から学習を始めるかどうか. はじめから訓練をする場合もTrueでok

# optimizer
lr: 0.001 # 学習率
adam_betas: [0.9, 0.999] # Adamのハイパーパラメーター
adam_weight_decay: 0.0 # Adamのハイパーパラメーター

# scheduler
steplr_step_size: 200000 # このステップごとに、lrをsteplr_gamma倍にする
steplr_gamma: 0.1
min_lr: 1e-06

# output
save_model_step_size: 1000 # このステップごとに、モデルを保存する
state_dict_dir: state_dicts # state_dictの保存先
save_frozen_model_dir: frozen_models # frozen_modelsの保存先

# allegro
# Allegroのconfig
model: Allegro
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
```
latents_to_edge_energy_hidden_dims: [128]
two_body_mlp_hidden_dims: [128]
env_embed_mlp_hidden_dims: []
latent_mlp_hidden_dims: [128]
activation_function: silu
avg_atom_energy: -5.0 # check_dataset.pyでavg_atom_energyがわかるので、それを指定する
