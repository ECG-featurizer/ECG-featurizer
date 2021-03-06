*****
Get started
*****
The purpose of this package is to make tabular data from ECG-recordings by calculating features. The package is built on WFDB [#]_ and NeuroKit2 [#]_. The package can be convenient when doing machine learning on ECG-data.

Usage:
=====

Featurize .dat-files:
^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    from ECGfeaturizer import featurize as ef

    # Make ECG-featurizer object
    Feature_object =ef.get_features()

    # Preprocess the data (filter, find peaks, etc.)
    My_features=Feature_object.featurizer_dat(features=ecg_filenames,labels=labels,directory="./data/",demographical_data=demo_data)

Featurize .mat-files:
^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    from ECGfeaturizer import featurize as ef

    number_of_ECGs = <the amount of ECGs>
    directory = "<your dir>"

    # Make ECG-featurizer object
    Feature_object =ef.get_features()

    # Preprocess the data (filter, find peaks, etc.)
    My_features=Feature_object.featurizer_mat(num_features=number_of_ECGs, mat_dir = directory)

Input data:
===========

**features:**
    A numpy array of ECG-recordings in directory. Each recording should have a file with the recording as a time series and one file with meta data containing information about    the patient and measurement information. This is standard format for WFDB and PhysioNet-files [1]_ [#]_  

    Supported input files:

    +-------------------+---------------------------+
    | **Input data**    | **Supported file format** |
    +-------------------+---------------------------+
    | ECG-recordings    | .dat files                |
    +-------------------+---------------------------+
    | Patient meta data | .hea files                |
    +-------------------+---------------------------+

**labels:**
    A numpy array of labels / diagnoses for each ECG-recording. The length of the labels-array should have the same length as the features-array
.. code-block:: python

        len(labels) == len(features)
    
**directory:**
    A string with the path to the features. If the folder structure looks like this:
    
    | mypath
    | ├── ECG-recordings          
    | │   ├── A0001.hea
    | │   ├── A0001.dat
    | │   ├── A0002.hea
    | │   ├── A0002.dat
    | │   └── Axxxx.dat
    
    then the feature and directory varaible could be:
    
.. code-block:: python
        features[0]
            "A0001"
        directory
            "./mypath/ECG-recordings/"
       
**demographical_data:**
    The demographical data that is used in this function is *age* and *gender*. A Dataframe with the following 3 columns should be passed to the featurizer() function.
    
    +---+---------+------------+-----------------+
    |   | **age** | **gender** | **filename_hr** |
    +===+=========+============+=================+
    | 0 | 11.0    | 1          | "A0001"         |
    +---+---------+------------+-----------------+
    | 1 | 57.0    | 0          | "A0002"         |
    +---+---------+------------+-----------------+
    | 2 | 94.0    | 0          | "A0003"         |
    +---+---------+------------+-----------------+
    | 3 | 34.0    | 1          | "A0004"         |
    +---+---------+------------+-----------------+
    
    The strings in the *filename_hr* -column should be the same as the strings in the feature array.
    In this example gender is OneHot encoded such that
    .. math::
        1 = Female 
        0 = Male
 

Output Features:
================
The features that are calculated in present version is:

+--------------------+--------------------+-----------------------------------------------------------------+
|  **Index number**  |  **Feature Name**  |  **Description**                                                |
+====================+====================+=================================================================+
| 0                  | gender             | Patients Gender                                                 |
+--------------------+--------------------+-----------------------------------------------------------------+
| 1                  | age                | Patients Age                                                    |
+--------------------+--------------------+-----------------------------------------------------------------+
| 2                  | R HR STD           | Heart rate standard deviation derived from R-peaks              |
+--------------------+--------------------+-----------------------------------------------------------------+
| 3                  | R HR median        | Heart rate median derived from R-peaks                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 4                  | R HR min           | Heart rate minimum derived from R-peaks                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 5                  | R HR max           | Heart rate maximum derived from R-peaks                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 6                  | R HR mean          | Heart rate mean derived from R-peaks                            |
+--------------------+--------------------+-----------------------------------------------------------------+
| 7                  | RMSSD              | Heart rate variability calculated using RMSSD                   |
+--------------------+--------------------+-----------------------------------------------------------------+
| 8                  | R amp II std       | Standard deviation of R-peak amplitude in lead II               |
+--------------------+--------------------+-----------------------------------------------------------------+
| 9                  | R amp II min       | Minimum R-peak amplitude in lead II                             |
+--------------------+--------------------+-----------------------------------------------------------------+
| 10                 | R amp II min_2     | *this was supposed to be R amp II max*                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 11                 | R amp leads I      | Voltage amplitude of R-peak in lead I                           |
+--------------------+--------------------+-----------------------------------------------------------------+
| 12                 | R amp leads II     | Voltage amplitude of R-peak in lead II                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 13                 | R amp lead III     | Voltage amplitude of R-peak in lead III                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 14                 | R amp lead aVR     | Voltage amplitude of R-peak in lead aVR                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 15                 | R amp lead aVL     | Voltage amplitude of R-peak in lead aVL                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 16                 | R amp lead aVF     | Voltage amplitude of R-peak in lead aVF                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 17                 | R amp V1           | Voltage amplitude of R-peak in lead V1                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 18                 | R amp V2           | Voltage amplitude of R-peak in lead V2                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 19                 | R amp V3           | Voltage amplitude of R-peak in lead V3                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 20                 | R amp V4           | Voltage amplitude of R-peak in lead V4                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 21                 | R amp V5           | Voltage amplitude of R-peak in lead V5                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 22                 | R amp V6           | Voltage amplitude of R-peak in lead V6                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 23                 | p_offset_std       | Standard deviation of heart rate calculated from P-offset       |
+--------------------+--------------------+-----------------------------------------------------------------+
| 24                 | p_offset_median    | Median heart rate calculated from P-offset                      |
+--------------------+--------------------+-----------------------------------------------------------------+
| 25                 | p_offset_min       | Minimum heart rate calculated from P-offset                     |
+--------------------+--------------------+-----------------------------------------------------------------+
| 26                 | p_offset_max       | Maximum heart rate calculated from P-offset                     |
+--------------------+--------------------+-----------------------------------------------------------------+
| 27                 | mean_p_offset      | Mean heart rate calculated from P-offset                        |
+--------------------+--------------------+-----------------------------------------------------------------+
| 28                 | p_onsets_std       | Standard deviation of heart rate calculated from P-onset        |
+--------------------+--------------------+-----------------------------------------------------------------+
| 29                 | p_onsets_median    | Median heart rate calculated from P-onset                       |
+--------------------+--------------------+-----------------------------------------------------------------+
| 30                 | p_onsets_min       | Minimum heart rate calculated from P-onset                      |
+--------------------+--------------------+-----------------------------------------------------------------+
| 31                 | p_onsets_max       | Maximum heart rate calculated from P-onset                      |
+--------------------+--------------------+-----------------------------------------------------------------+
| 32                 | mean_p_onsets      | Mean heart rate calculated from P-onset                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 33                 | ECG_baseline       | ECG baseline calculated taking the mean of all P-onset voltages |
+--------------------+--------------------+-----------------------------------------------------------------+
| 34                 | p_rate_std         | Standard deviation of heart rate calculated from P-peak         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 35                 | p_rate_median      | Median heart rate calculated from P-peak                        |
+--------------------+--------------------+-----------------------------------------------------------------+
| 36                 | p_rate_min         | Minimum heart rate calculated from P-peak                       |
+--------------------+--------------------+-----------------------------------------------------------------+
| 37                 | p_rate_max         | Maximum heart rate calculated from P-peak                       |
+--------------------+--------------------+-----------------------------------------------------------------+
| 38                 | mean_p_rate        | Mean heart rate calculated from P-peak                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 39                 | P amp leads I      | Voltage amplitude of P-peak in lead I                           |
+--------------------+--------------------+-----------------------------------------------------------------+
| 40                 | P amp leads II     | Voltage amplitude of P-peak in lead II                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 41                 | P amp lead III     | Voltage amplitude of P-peak in lead III                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 42                 | P amp lead aVR     | Voltage amplitude of P-peak in lead aVR                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 43                 | P amp lead aVL     | Voltage amplitude of P-peak in lead aVL                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 44                 | P amp lead aVF     | Voltage amplitude of P-peak in lead aVF                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 45                 | P amp V1           | Voltage amplitude of P-peak in lead V1                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 46                 | P amp V2           | Voltage amplitude of P-peak in lead V2                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 47                 | P amp V3           | Voltage amplitude of P-peak in lead V3                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 48                 | P amp V4           | Voltage amplitude of P-peak in lead V4                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 49                 | P amp V5           | Voltage amplitude of P-peak in lead V5                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 50                 | P amp V6           | Voltage amplitude of P-peak in lead V6                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 51                 | q_rate_std         | Standard deviation of heart rate calculated from Q-peak         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 52                 | q_rate_median      | Median heart rate calculated from Q-peak                        |
+--------------------+--------------------+-----------------------------------------------------------------+
| 53                 | q_rate_min         | Minimum heart rate calculated from Q-peak                       |
+--------------------+--------------------+-----------------------------------------------------------------+
| 54                 | q_rate_max         | Maximum heart rate calculated from Q-peak                       |
+--------------------+--------------------+-----------------------------------------------------------------+
| 55                 | mean_q_rate        | Mean heart rate calculated from Q-peak                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 56                 | Q amp leads I      | Voltage amplitude of Q-peak in lead I                           |
+--------------------+--------------------+-----------------------------------------------------------------+
| 57                 | Q amp leads II     | Voltage amplitude of Q-peak in lead II                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 58                 | Q amp lead III     | Voltage amplitude of Q-peak in lead III                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 59                 | Q amp lead aVR     | Voltage amplitude of Q-peak in lead aVR                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 60                 | Q amp lead aVL     | Voltage amplitude of Q-peak in lead aVL                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 61                 | Q amp lead aVF     | Voltage amplitude of Q-peak in lead aVF                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 62                 | Q amp V1           | Voltage amplitude of Q-peak in lead V1                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 63                 | Q amp V2           | Voltage amplitude of Q-peak in lead V2                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 64                 | Q amp V3           | Voltage amplitude of Q-peak in lead V3                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 65                 | Q amp V4           | Voltage amplitude of Q-peak in lead V4                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 66                 | Q amp V5           | Voltage amplitude of Q-peak in lead V5                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 67                 | Q amp V6           | Voltage amplitude of Q-peak in lead V6                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 68                 | s_rate_std         | Standard deviation of heart rate calculated from S-peak         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 69                 | s_rate_median      | Median heart rate calculated from S-peak                        |
+--------------------+--------------------+-----------------------------------------------------------------+
| 70                 | s_rate_min         | Minimum heart rate calculated from S-peak                       |
+--------------------+--------------------+-----------------------------------------------------------------+
| 71                 | s_rate_max         | Maximum heart rate calculated from S-peak                       |
+--------------------+--------------------+-----------------------------------------------------------------+
| 72                 | mean_s_rate        | Mean heart rate calculated from S-peak                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 73                 | S amp leads I      | Voltage amplitude of S-peak in lead I                           |
+--------------------+--------------------+-----------------------------------------------------------------+
| 74                 | S amp leads II     | Voltage amplitude of S-peak in lead II                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 75                 | S amp lead III     | Voltage amplitude of S-peak in lead III                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 76                 | S amp lead aVR     | Voltage amplitude of S-peak in lead aVR                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 77                 | S amp lead aVL     | Voltage amplitude of S-peak in lead aVL                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 78                 | S amp lead aVF     | Voltage amplitude of S-peak in lead aVF                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 79                 | S amp V1           | Voltage amplitude of S-peak in lead V1                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 80                 | S amp V2           | Voltage amplitude of S-peak in lead V2                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 81                 | S amp V3           | Voltage amplitude of S-peak in lead V3                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 82                 | S amp V4           | Voltage amplitude of S-peak in lead V4                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 83                 | S amp V5           | Voltage amplitude of S-peak in lead V5                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 84                 | S amp V6           | Voltage amplitude of S-peak in lead V6                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 85                 | t_rate_std         | Standard deviation of heart rate calculated from T-peak         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 86                 | t_rate_median      | Median heart rate calculated from T-peak                        |
+--------------------+--------------------+-----------------------------------------------------------------+
| 87                 | t_rate_min         | Minimum heart rate calculated from T-peak                       |
+--------------------+--------------------+-----------------------------------------------------------------+
| 88                 | t_rate_max         | Maximum heart rate calculated from T-peak                       |
+--------------------+--------------------+-----------------------------------------------------------------+
| 89                 | mean_t_rate        | Mean heart rate calculated from T-peak                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 90                 | T amp leads I      | Voltage amplitude of T-peak in lead I                           |
+--------------------+--------------------+-----------------------------------------------------------------+
| 91                 | T amp leads II     | Voltage amplitude of T-peak in lead II                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 92                 | T amp lead III     | Voltage amplitude of T-peak in lead III                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 93                 | T amp lead aVR     | Voltage amplitude of T-peak in lead aVR                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 94                 | T amp lead aVL     | Voltage amplitude of T-peak in lead aVL                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 95                 | T amp lead aVF     | Voltage amplitude of T-peak in lead aVF                         |
+--------------------+--------------------+-----------------------------------------------------------------+
| 96                 | T amp V1           | Voltage amplitude of T-peak in lead V1                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 97                 | T amp V2           | Voltage amplitude of T-peak in lead V2                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 98                 | T amp V3           | Voltage amplitude of T-peak in lead V3                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 99                 | T amp V4           | Voltage amplitude of T-peak in lead V4                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 100                | T amp V5           | Voltage amplitude of T-peak in lead V5                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 101                | T amp V6           | Voltage amplitude of T-peak in lead V6                          |
+--------------------+--------------------+-----------------------------------------------------------------+
| 102                | t_offset_std       | Standard deviation of heart rate calculated from T-offset       |
+--------------------+--------------------+-----------------------------------------------------------------+
| 103                | t_offset_median    | Median heart rate calculated from T-offset                      |
+--------------------+--------------------+-----------------------------------------------------------------+
| 104                | t_offset_min       | Minimum heart rate calculated from T-offset                     |
+--------------------+--------------------+-----------------------------------------------------------------+
| 105                | t_offset_max       | Maximum heart rate calculated from T-offset                     |
+--------------------+--------------------+-----------------------------------------------------------------+
| 106                | mean_t_offset      | Mean heart rate calculated from T-offset                        |
+--------------------+--------------------+-----------------------------------------------------------------+
| 107                | t_onsets_std       | Standard deviation of heart rate calculated from T-onset        |
+--------------------+--------------------+-----------------------------------------------------------------+
| 108                | t_onsets_median    | Median heart rate calculated from T-onset                       |
+--------------------+--------------------+-----------------------------------------------------------------+
| 109                | t_onsets_min       | Minimum heart rate calculated from T-onset                      |
+--------------------+--------------------+-----------------------------------------------------------------+
| 110                | t_onsets_max       | Maximum heart rate calculated from T-onset                      |
+--------------------+--------------------+-----------------------------------------------------------------+
| 111                | mean_t_onsets      | Mean heart rate calculated from T-onset                         |
+--------------------+--------------------+-----------------------------------------------------------------+

References:
===========

.. [#] WFDB: https://github.com/MIT-LCP/wfdb-python
.. [#] Makowski, D., Pham, T., Lau, Z. J., Brammer, J. C., Lesspinasse, F., Pham, H.,
  Schölzel, C., & S H Chen, A. (2020). NeuroKit2: A Python Toolbox for Neurophysiological
  Signal Processing. Retrieved March 28, 2020, from https://github.com/neuropsychology/NeuroKit
.. [#] Goldberger AL, Amaral LAN, Glass L, Hausdorff JM, Ivanov PCh, Mark RG, Mietus JE, Moody GB, Peng CK, Stanley HE. PhysioBank, PhysioToolkit, and PhysioNet: Components of a New Research Resource for Complex Physiologic Signals. Circulation 101(23):e215-e220 [Circulation Electronic Pages; http://circ.ahajournals.org/content/101/23/e215.full]; 2000 (June 13). PMID: 10851218; doi: 10.1161/01.CIR.101.23.e215
