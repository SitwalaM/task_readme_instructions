## Clustering Data Using X and Y Coordinates

Scripts in `src` cluster data using x and y coordinates HDBSCAN algorithm.

> **Note:** All scripts should be run from the `src` folder.

### Quick Usage

1. **Setup Config**
   - Run: `python3 setup_config.py --folder <path_to_gz_files> --batch_size <size>`
   - `--folder`: Folder containing `.gz` files to process.
   - `--batch_size`: Number of files per SLURM array job.
   - Generates `batch_config.csv` for SLURM array lookup.
   - Output path (default: `../data/clusters`) can be changed in the config or script.
   - Batch size and number of jobs are set by the arguments above.

2. **Run SLURM Array**
   - The SLURM array is currently set up for a batch size of 10 and array range 1-6.
   - Edit the `.sh` script to change the array range, e.g. `#SBATCH --array=1-6` (see terminal output from setup_config.py for correct range).
   - Submit the job: `sbatch parallel_hdb_scan.sh`

> **Quick Run:** For default settings, simply run:
> ```bash
> python3 setup_config.py
> sbatch parallel_hdb_scan.sh
> ```

3. **Outputs**
   - Cluster results are saved in `../data/clusters`.
   - Each output folder contains:
     - `readme.txt` with the number of clusters per file
     - Images of clusters for each file
     - Compressed CSV with cluster number per data_id
   - Failed files are listed in `error_log.txt` (e.g., files with only zeros or other issues).

> **Storage Note:** Budget for about 100 KB of space in the output folder for each data_id. This should be considered if processing a very large number of files.

## Memory Estimates and Considerations

- **Time Estimate:**
  - Base estimate for 100 batch_size: ~16 min (slowest task: 9.7 sec/file)
  - Recommended for 100 batch_size: `#SBATCH --time=00:25:00` (25 min, ~55% safety margin)
 
- **Memory Estimate:**
  - Peak for 10 files: ~195 MB
  - For 100 files: ~195 MB x 10 = 1950 MB ≈ 2 GB
  - Recommended: `#SBATCH --mem=4G` (request at least 4 GB for safety)


