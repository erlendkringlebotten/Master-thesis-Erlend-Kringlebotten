# Handling data from spectroradiometer.py

This Python script contains five functions. The first function is 
# `read_spectroradiometer_data` 
This reads data from a CSV file containing spectral irradiance measurements from a spectroradiometer. It returns a pandas DataFrame with the measurements and calculates the Average Photon Energy (APE) for instances with over 25W/m2 integrated irradiance if `APE=True`.

## Function Usage

```python
read_spectroradiometer_data(file_path, APE=False, irradiation_threshold=25)
```

### Parameters

- `file_path`: The path to the CSV file.
- `APE`: A boolean that determines whether to calculate the APE. Default is `False`.
- `irradiation_threshold`: The irradiance threshold for which APEs are calculated. Default is `25`.

### Returns

- If `APE=False`, it returns a DataFrame with timestamp and measurement per wavelength.
- If `APE=True`, it returns two DataFrames. The first one is the same as above, and the second one contains irradiance [W/m2] and APE for each minute.

## Dependencies

This script requires the following libraries:

- pandas
- numpy
- matplotlib.pyplot
- scipy.integrate
- datetime
- copy

## Example

```python
df = read_spectroradiometer_data('path_to_your_file.csv', APE=True, irradiation_threshold=30)
```

This will return two DataFrames. The first DataFrame contains the spectral irradiance measurements. The second DataFrame contains the calculated irradiance and APE for instances where the calculated irradiance for the entire spectrum is greater than 30W/m2.

## Error Handling

The function checks if there are any NA values in the resulting DataFrame(s) and raises a ValueError if there are.

## Note

The function adjusts the timestamps for a time drift in the measurements for certain dates.

The second function is: 
# collect_data_from_directory

This Python script contains a function `collect_data_from_directory` that iterates over all CSV files in a given directory, reads spectral irradiance measurements from each file using the `read_spectroradiometer_data` function, and returns a DataFrame with all the measurements. If `APE=True`, it also returns an additional DataFrame with the Average Photon Energy (APE) values and irradiance.

## Function Usage

```python
collect_data_from_directory(directory_path, APE=False)
```

### Parameters

- `directory_path`: The path to the directory containing the CSV files.
- `APE`: A boolean that determines whether to calculate the APE. Default is `False`.

### Returns

- If `APE=False`, it returns a DataFrame with all the measurements from all CSV files in the directory.
- If `APE=True`, it returns two DataFrames. The first one contains all the measurements, and the second one contains the APE values and irradiance for each file.

## Dependencies

This script requires the following libraries:

- os
- pandas

It also requires the `read_spectroradiometer_data` function from the same script.

## Example

```python
df = collect_data_from_directory('path_to_your_directory', APE=True)
```

This will return two DataFrames. The first DataFrame contains all the spectral irradiance measurements from all CSV files in the directory. The second DataFrame contains the calculated APE values and irradiance for each file.
## Note

The function only processes CSV files in the directory. It checks if a file is a CSV file and if it exists before processing it.

The third function is:
# plotting_av_spektroradiometer_data

This function plots spectral irradiance data from a DataFrame. It can plot data for multiple dates and times, include irradiance in the plot, and include uncertainty as a shaded area.

## Function Usage

```python
plotting_av_spektroradiometer_data(data, dato, klokkeslett, inkludere_innstråling=False, legend_loc="upper left", legend=True, usikkerhet=False, set_legend=False)
```

### Parameters

- `data`: A DataFrame with wavelengths and timestamps.
- `dato`: A list of dates in the format YYYY-MM-DD.
- `klokkeslett`: A list of times in the format HH:MM:SS.
- `inkludere_innstråling`: If True, the irradiance is calculated based on the 300-1100 nm range and printed in the plot. Default is `False`.
- `legend_loc`: The location of the legend in the plot. Default is "upper left".
- `legend`: If True, a legend is included in the plot. Default is `True`.
- `usikkerhet`: If True, the uncertainty is included as a shaded area in the plot. Default is `False`.
- `set_legend`: If set, this will be used as the legend label. Default is `False`.

### Returns

This function does not return any value. It plots the spectral irradiance data.

## Dependencies

This script requires the following libraries:

- matplotlib.pyplot
- scipy.integrate
- pandas

## Example

```python
plotting_av_spektroradiometer_data(df, ['2022-01-01'], ['12:00:00'], inkludere_innstråling=True, usikkerhet=True)
```

This will plot the spectral irradiance data from the DataFrame `df` for the date 2022-01-01 at 12:00:00. The plot will include irradiance and uncertainty.

## Note

The function calculates irradiance using the trapezoidal rule for numerical integration. The uncertainty is calculated based on predefined wavelength ranges and their corresponding uncertainty values.



The fourth function is: 
# beregne_innstråling

This Python script contains a function `beregne_innstråling` that calculates the irradiation for each specified date and time from a DataFrame containing spectral irradiance data. The results are returned as a new DataFrame.

## Function Usage

```python
beregne_innstråling(dataframe, dato, klokkeslett)
```

### Parameters

- `dataframe`: A DataFrame with spectral irradiance data.
- `dato`: A list of dates in the format YYYY-MM-DD.
- `klokkeslett`: A list of times in the format HH:MM:SS.

### Returns

A DataFrame with the calculated irradiation for each specified date and time. The DataFrame has two columns: "Date and time" and "Irradiation [W/m2]".

## Dependencies

This script requires the following libraries:

- scipy.integrate
- pandas

## Example

```python
df_irradiation = beregne_innstråling(df, ['2022-01-01'], ['12:00:00'])
```

This will calculate the irradiation for the date 2022-01-01 at 12:00:00 from the DataFrame `df` and return a new DataFrame with the results.

## Note

The function calculates irradiation using the trapezoidal rule for numerical integration. The irradiation is calculated for each specified date and time and added to the new DataFrame. The function returns the new DataFrame with all the calculated irradiation values.

The last function is:
# snitt_av_spektroradiometer

This script calculates the mean of the spectral irradiance data in a DataFrame for a specified time range. The results are returned as a new DataFrame.

## Function Usage

```python
snitt_av_spektroradiometer(input_df, start_time, end_time)
```

### Parameters

- `input_df`: A DataFrame with spectral irradiance data.
- `start_time`: The start of the time range in the format YYYY-MM-DD HH:MM:SS.
- `end_time`: The end of the time range in the same format as `start_time`.

### Returns

A DataFrame with the mean spectral irradiance values for the specified time range. The DataFrame has one column named "Mean from `start_time` to `end_time`" and the same index as `input_df`.

## Dependencies

This script requires the pandas library.

## Example

```python
df_mean = snitt_av_spektroradiometer(df, '2022-01-01 12:00:00', '2022-01-01 13:00:00')
```

This will calculate the mean spectral irradiance for the time range from 12:00:00 to 13:00:00 on 2022-01-01 from the DataFrame `df` and return a new DataFrame with the results.

## Note

The function converts the start and end times to datetime objects and selects the columns in `input_df` that fall within the specified time range. It then calculates the mean of these columns and creates a new DataFrame with the mean values. If no columns are found within the time range, the function raises a ValueError.
