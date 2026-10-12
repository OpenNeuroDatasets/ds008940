# EEGLAB conversion script

`edf_to_eeglab.m` regenerates the EEGLAB `.set` files in `derivatives/eeglab/`
from the canonical BIDS EDF files at the repository root.

## Why this exists
EEGLAB's native EDF import does not reliably parse the event labels, so the
`.set` files store them as strings. They are provided ready-made in
`derivatives/eeglab/`; this script documents how they were made and lets you
rebuild them.

## Requirements
- MATLAB-based EEGLAB (the standalone/compiled EEGLAB cannot run scripts).
- The BIOSIG plugin (EEGLAB > File > Manage extensions > BIOSIG).

## Usage
1. In MATLAB: `>> eeglab`
2. Set `BIDS_ROOT` at the top of `edf_to_eeglab.m` to the dataset root
   (the folder containing `sub-01`, `sub-02`, …), or `cd` there first.
3. `>> edf_to_eeglab`

Output: `derivatives/eeglab/sub-NN_task-foodcat_eeg.set`, one per subject.
The 14 EEG channels are the data channels; the EMOTIV quality channels
(`CQ_*`, `EQ_*`) are stored in `EEG.etc.quality` (`labels`, `data`).
Events come from `events.tsv`, including `boundary` events at the two segment
joins, which EEGLAB treats as discontinuities when filtering and epoching.
No filtering, resampling, or re-referencing is applied.

## Notes
- Users of MNE-Python or FieldTrip can work directly from the EDF + events.tsv
  and do not need this script.
