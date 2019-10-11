University of Alabama CERN Hackathon
=========================================
mknipfer

- download [starter kit](https://github.com/iml-wg/lhcmlhackathon)
- Install [root](https://root.cern.ch/root-files) (sudo pacman -Syu root), download data
- do what is written in the startet kit. Takes a very long time on my old thinkpad
- need tensorflow 1.14 for this. Version 2.0 does not work because it removed a function
- built tensorflow from [source](https://www.tensorflow.org/install/source) for MAVX support (ok I failed and didn't do this. Not enough time to waste on this)
- Since root is the devil's work and I am just not smart enough to understand that tree stuff I switched to pandas


## Data Visualization
- there are what I call raw colums and manual columns
- Pair plots
    * take a long time to plot
    * see that data is rather hard
    * maybe go to polar coordinates for some variables?
- parallel coordinate plots
    * go for only a sample of 500, otherwise too much. Maybe do same for pairplot
    * there is some structure in the data

## Simple Algos
- AUC = area under roc curve. This is what will be used to find the winner
- Example Notebook (Keras_Dens): AUC = 0.76
- Logistic regression
    * raw colums: AUC = 0.57
    * man colums: AUC = 0.71
- KNN: okay
- Random Forest
    * from [here](https://towardsdatascience.com/an-implementation-and-explanation-of-the-random-forest-in-python-77bf308a9b76<Paste>)
	* actually as good as NN as far as I can tell
	* idea: try combination of NN and RF

# ToDo
- Install Tensorflow with AVX2 FMA
- Try Random Forest Output as Input for NN
- Other models?


# BEST MODEL WITH PARAMETERS
use_cols = man_cols + ['lepton_pT', 'missing_energy_magnitude', 'jet1_pt', 'jet2_pt'] + [
    'Z', 'Eangle']

model = Sequential()
model.add(Dense(256, kernel_initializer='glorot_normal', activation='linear', input_dim=input_dim))
model.add(BatchNormalization())

model.add(Dense(256, kernel_initializer='glorot_normal', activation='relu'))
model.add(Dense(256, kernel_initializer='glorot_normal', activation='linear'))
model.add(BatchNormalization())

model.add(Dense(256, kernel_initializer='glorot_normal', activation='relu'))
model.add(Dense(256, kernel_initializer='glorot_normal', activation='linear'))
model.add(BatchNormalization())

model.add(Dense(256, kernel_initializer='glorot_normal', activation='relu'))
model.add(Dense(256, kernel_initializer='glorot_normal', activation='linear'))
model.add(BatchNormalization())

model.add(Dense(256, kernel_initializer='glorot_normal', activation='relu'))
model.add(Dropout(0.2))

model.add(Dense(2, kernel_initializer='glorot_uniform', activation='softmax'))
model.compile(loss='categorical_crossentropy', optimizer=Adam(), metrics=['categorical_accuracy',])
model.save('model_dense.h5')
model.summary()
