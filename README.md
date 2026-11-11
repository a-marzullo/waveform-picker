# Interactive Acoustic Emission (AE) Signal Picker

## 1. What the Code Does

This Python code provides an interactive tool for manually identifying the arrival times of Acoustic Emission (AE) signals recorded by multiple sensors.

The code displays the waveform of one channel at a time, allowing the user to:

- **Select arrival times** by left-clicking on the waveform. A red dashed line and cross indicate the selected time.
- **Modify or delete picks** by left-clicking on a different position or right-clicking to remove the existing pick.
- **Navigate between channels** using the Next and Prev buttons or keyboard shortcuts (`N` and `P`).
- **Apply frequency filters** by specifying high-pass (HP) and low-pass (LP) cutoff frequencies in kHz.
- **Save waveform plots** as PNG images.
- **Print the selected arrival times** for all channels.

The selected arrival times are stored while navigating between channels, allowing the user to review or modify previous selections.


## 2. Input Variables

To run the code, the user must provide the waveform data, sampling frequency, and optionally the channel identifiers and previously selected arrival times.

The main function is called as follows:

```python
my_channels = [0, 1, 2, 3, 4, 5]
output_dir = '/home/user/Desktop/pick'
picker = plot_single_channel_picking(
    data=data_chunk[my_channels, :],
    fs=fsAE,
    channel_ids=my_channels,
    initial_picks=raw_picks
)
```

### Description of the variables

| Variable | Description | Example |
|---|---|---|
| `data_chunk` | NumPy array containing the recorded AE signals. Each row represents one channel and each column represents a time sample. | Array with shape `(29, 10000)` |
| `my_channels` | List of channel IDs to visualize and analyze. | `[0, 1, 2, 3, 4, 5]` |
| `fsAE` | Sampling frequency of the AE acquisition system, expressed in Hz. | `5_000_000` (5 MHz) |
| `raw_picks` | Optional list of previously identified arrival times. If no picks are available, use `None`. | `[{"ch": 0, "time": 12.5}]` |

**Notes:**

- The waveform data and sampling frequency must already be loaded into the Jupyter Notebook.
- `data_chunk[my_channels, :]` selects only the signals corresponding to the specified channels.
- Initial arrival times (`raw_picks`) must be expressed in microseconds (µs).
- If no previous picks are available, set `initial_picks=None`.
- The high-pass and low-pass filtering frequencies can be modified directly in the interactive figure.

Before running the function, set `output_dir` inside `plot_single_channel_picking()` to a valid directory, such as `"./saved_plots"`.

## 3. Output

An example of the resulting interface:

!["Example of the UI"](https://github.com/a-marzullo/waveform-picker/blob/main/manual_picker%20(1).png)


After selecting the arrival times for the desired channels, the results can be retrieved using:

```python
event_dict = picker.print_picks_to_console()
```

The code prints a list of dictionaries containing the selected arrival time and its corresponding channel ID.

### Example output

```python
--- Final Picks List ---
[
    {'time': 0.00001250, 'ch': 0},
    {'time': 0.00001420, 'ch': 1},
    {'time': 0.00001380, 'ch': 2},
    {'time': 0.00001510, 'ch': 3},
    {'time': 0.00001190, 'ch': 4}
]
```

Each dictionary contains two values:

| Key | Description |
|---|---|
| `time` | Selected arrival time, expressed in **seconds (s)**. |
| `ch` | ID of the acquisition channel associated with that arrival time. |

For example:

```python
{'time': 0.00001250, 'ch': 0}
```

indicates that the arrival time identified for **channel 0 is 12.5 µs** (0.00001250 seconds).

**Final notes:**

- The arrival times are selected on the plot in microseconds, the code automatically converts them into seconds when printing the results.
- Only channels for which an arrival time has been selected are included in the output.
- The results are also stored in the variable `event_dict` and can be used for subsequent analysis, such as AE source localization.
- The "Save Plot (PNG)" button allows the user to save the currently displayed waveform together with its selected arrival-time marker in a chosen directory.