# Formant tracking (dynamic seeding)

  This is a script that incorporates seeding method (Chen et al. 2019) and dynamic reference values to extract more accurate formants from vocalic segments. This is a Praat script that I wrote to process data of my dissertation: *Prosodic conditions on the acoustics of vowel sequences*.

  Since my study investigates the formant transition in complex vocalic segments (e.g., diphthongs/triphthongs), the usual method of setting one set of reference formant values for each vocalic segment is not ideal. And also because there are several different vocalic segments in my recordings (both monophthongs and diphthongs), manually setting formant reference values for each vowel and each speaker is a tedious process. Therefore, I decided to incorporate both seeding method and dynamic formant reference setting into one single Praat script that loops through all files in all subdirectories in a folder, which supposedly should make formant tracking faster and more accurate.

## Reference files

  There are **THREE** reference `.csv` files you need to prepare before running the script:

  1. A `.csv` file that has `formant_ceiling` values and `number of formants` for different `gender` (usually F(emale) and M(ale)).

  This file should exactly have 3 columns: `gender`, `formant_ceiling` and `number_of_formants`. The formant ceiling and number of formants to track should differ according to the gender. Usually I set ceiling to 5500Hz for females and 4500Hz or even 4000Hz for males. The number of formants to track should also be different by gender (5 for females and 4 for males).

  2. A `.csv` file that logs the `speaker_id` and their `gender`.

  This file should have exactly 2 columns: `speaker_id` and `gender`. The `speaker_id` should use the same strings you use for the names of the folders of different speakers. Which means if your first speaker's recordings are in a folder called '**sp_1**', then in the file its id should also be '**sp_1**'.

  3. A `.csv` file that contains the `reference values (F1-F3)` for the vowels you are going to extract formants from in your recordings. The formant reference values should be set for both genders (and children, if there are any).

  This file should have exactly `2+9=11` columns: `vowel`, `gender`, and 9 other columns for `formant reference values`. In your formant reference file you should specify reference F1-F3 values separately for the **initial, medial, and final 33%** of the vowel (3 formants for each tertile, and 9 reference values for each vowel in total). For monophthongs, set the reference values of the initial and final reference values to 0. My script will equate the initial and final reference  to the medial reference values for monophthongs automatically. The formant reference file **must contain all the segment labels** you used in your textgrid file, otherwise the script will break once it can't find the target vowel in the formant reference file.

## How to use

The script allows you to specify another tier (shown as `Syllable tier number` in the form window) that may contain syllable information or other sort. If you don't want to extract additional information from another tier, simply set it to `0`.

After running the script, it will first ask you to choose the `speaker log` file, the `formant reference` file and `formant setting` file. Then it will ask you to choose `the folder` where you save your recordings. Note that since the script loops through subdirectories in a folder and look for the subfolder names as speakers' ids, all your recordings should be saved in separate folders for each speaker.

## The logic of the script

The script first reads in the three reference files as tables and then loop through all the sound and textgrid fils in all the subdirectories.

The script will first identify the speaker by get the name of the folder and then find the speaker's gender from the speaker log file. Then it will use the gender information to locate the formant ceiling, number of formants to track, and formant reference values.

During formant extraction, the script divides datapoints, the number of which is specified by the use in the form, into three tertiles: the first, middle and last 33% of the vowel. The it will use different formant reference values to track the formant.

After up to 4 formants are obtained, the results will be output to two log files.

## What do you get

The script will output two files.

One file logs the contextual information, the previous and subsequent segments around the target vowel, the label of the current syllable and the syllable duration.

The other file will log in `F1-F4` values from several equidistant intervals of each vowel as specified by the user in the form before running the script. This file will also log four spectral moment: `center of gravity`, `standard deviation`, `skewness` and `kurtosis`.

## The advantages of this script

1. It's fast. By using this script, it speeds up your process of obtaining data. By setting formant values for gender and different vowels all at once, the users no longer need to repetitively run script for different vowels and different speakers separately.
2. It's accurate. By setting different formant reference values for different vowels in different tertiles (for vowel sequence), it allows more accurate formant tracking than using one set of formant reference values across time and different vowels.
