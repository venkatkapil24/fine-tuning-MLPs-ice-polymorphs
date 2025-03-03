# Fine-tuned MLPs for ice polymorphs
Repository containing data and code for fine-tuned models for ice polymorphs

## Contents

| **Directory** | **Description**                                                         |
|---------------|-------------------------------------------------------------------------|
| dft_data      | Contains DFT total energy, force, and stress data for four ice polymorphs |
| models      | Contains MACE fine-tuned models for four ice polymorphs. The first number in the naming convention indicates the number of structures used to train the model. |


## Training scripts

### From-scratch training

```bash
python ${MACE_ROOT}/mace/scripts/run_train.py \
    --name="ice_MACE_model" \
    --model="ScaleShiftMACE" \
    --hidden_irreps="128x0e + 128x1o + 128x2e" \
    --num_radial_basis=8 \
    --num_cutoff_basis=5 \
    --default_dtype='float64' \
    --r_max=6.0 \
    --valid_fraction=0.1 \
    --E0s='average' \
    --batch_size=1 \
    --valid_batch_size=1 \
    --loss="stress" \
    --max_num_epochs=1000 \
    --energy_weight=10000 \
    --forces_weight=550 \
    --stress_weight=1 \
    --swa \
    --start_swa=500 \
    --swa_energy_weight=10000 \
    --swa_forces_weight=550 \
    --swa_stress_weight=1 \
    --swa_lr=0.00025 \
    --ema \
    --ema_decay=0.99 \
    --amsgrad \
    --default_dtype="float64" \
    --restart_latest \
    --device="cuda" \
    --save_cpu \
    --train_file="" \
    --test_file="" \
    --energy_key="REF_energy" \
    --forces_key="REF_forces" \
    --stress_key="REF_stress" \
    --valid_fraction=0.1 \
    --error_table="PerAtomRMSE"
```

### Fine-tuning from MACE-MP-0

```bash 
python ${MACE_ROOT}/mace/scripts/run_train.py \
    --name="ice_MACE_model" \
    --foundation_model="large" \
    --default_dtype='float32' \
    --r_max=6.0 \
    --valid_fraction=0.1 \
    --E0s="average" \
    --loss="stress" \
    --train_file="" \
    --test_file="" \
    --scaling="rms_forces_scaling" \
    --error_table="PerAtomMAE" \
    --batch_size=2 \
    --max_num_epochs=1000 \
    --forces_weight=550 \
    --energy_weight=10000 \
    --swa \
    --start_swa=500 \
    --swa_forces_weight=550 \
    --swa_energy_weight=10000 \
    --swa_stress_weight=5 \
    --swa_lr=0.00025 \
    --ema \
    --ema_decay=0.99 \
    --amsgrad \
    --restart_latest \
    --device=cuda \
    --seed=1
```

