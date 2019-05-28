# Code Disection


### Custom Callback class
* For calculating various evaluation metrics at the end of each epoch.
* on_train_begin, on_epoch_end are default functions called by callback function. So name of the functions must be same.

```python

class Metrics(Callback):
  def on_train_begin(self, logs={}):
    self.val_f1s = []
    self.val_recalls = []
    self.val_precisions = []
 
  def on_epoch_end(self, epoch, logs={}):
    val_predict = (np.asarray(self.model.predict([self.model.validation_data[0],self.model.validation_data[1],self.model.validation_data[2],self.model.validation_data[3],self.model.validation_data[4],self.model.validation_data[5]]))).round()
    val_targ = self.model.validation_data[6]
    _val_f1 = f1_score(val_targ, val_predict)
    _val_recall = recall_score(val_targ, val_predict)
    _val_precision = precision_score(val_targ, val_predict)
    self.val_f1s.append(_val_f1)
    self.val_recalls.append(_val_recall)
    self.val_precisions.append(_val_precision)
    print " — val_f1: %f — val_precision: %f — val_recall %f" %(_val_f1, _val_precision, _val_recall)
    return

metrics = Metrics()
```
* An object of the class needs to be passed to callback function while training.


### Importing Data

```python
data = pd.read_csv('project_stuff/clean_dataset.csv')
y = data.is_duplicate.values

tk = text.Tokenizer(nb_words=200000)

max_len = 40
tk.fit_on_texts(list(data.question1.values.astype(str)) + list(data.question2.values.astype(str)))
x1 = tk.texts_to_sequences(data.question1.values.astype(str))
x1 = sequence.pad_sequences(x1, maxlen=max_len)

x2 = tk.texts_to_sequences(data.question2.values.astype(str))
x2 = sequence.pad_sequences(x2, maxlen=max_len)
word_index = tk.word_index

print ('data imported')
```

* Imporing csv file and adding padding to make length 40.


### Deserializing the serialized GloVe vectors

```python
with open(r"project_stuff/embeddings.pickle", "rb") as input_file:
  embedding_matrix = cPickle.load(input_file)
```

## Neural network

### Deep NN

```python
model1 = Sequential()
model1.add(Embedding(len(word_index) + 1,
                     300,
                     weights=[embedding_matrix],
                     input_length=40,
                     trainable=False))

model1.add(TimeDistributed(Dense(300, activation='relu')))
model1.add(Lambda(lambda x: K.sum(x, axis=1), output_shape=(300,)))

model2 = Sequential()
model2.add(Embedding(len(word_index) + 1,
                     300,
                     weights=[embedding_matrix],
                     input_length=40,
                     trainable=False))

model2.add(TimeDistributed(Dense(300, activation='relu')))
model2.add(Lambda(lambda x: K.sum(x, axis=1), output_shape=(300,)))


merged_model = Sequential()
merged_model.add(Merge([model1, model2], mode='concat'))
merged_model.add(BatchNormalization())

merged_model.add(Dense(300))
merged_model.add(PReLU())
merged_model.add(Dropout(0.2))
merged_model.add(BatchNormalization())

merged_model.add(Dense(200))
merged_model.add(PReLU())
merged_model.add(Dropout(0.2))
merged_model.add(BatchNormalization())

merged_model.add(Dense(100))
merged_model.add(PReLU())
merged_model.add(Dropout(0.2))
merged_model.add(BatchNormalization())

merged_model.add(Dense(1))
merged_model.add(Activation('sigmoid'))
```

### CNN

```python
model3 = Sequential()
model3.add(Embedding(len(word_index) + 1,
                     300,
                     weights=[embedding_matrix],
                     input_length=40,
                     trainable=False))
model3.add(Convolution1D(nb_filter=nb_filter,
                         filter_length=filter_length,
                         border_mode='valid',
                         activation='relu',
                         subsample_length=1))
model3.add(Dropout(0.2))

model3.add(Convolution1D(nb_filter=nb_filter,
                         filter_length=filter_length,
                         border_mode='valid',
                         activation='relu',
                         subsample_length=1))

model3.add(GlobalMaxPooling1D())
model3.add(Dropout(0.2))

model3.add(Dense(300))
model3.add(Dropout(0.2))
model3.add(BatchNormalization())

model4 = Sequential()
model4.add(Embedding(len(word_index) + 1,
                     300,
                     weights=[embedding_matrix],
                     input_length=40,
                     trainable=False))
model4.add(Convolution1D(nb_filter=nb_filter,
                         filter_length=filter_length,
                         border_mode='valid',
                         activation='relu',
                         subsample_length=1))
model4.add(Dropout(0.2))

model4.add(Convolution1D(nb_filter=nb_filter,
                         filter_length=filter_length,
                         border_mode='valid',
                         activation='relu',
                         subsample_length=1))

model4.add(GlobalMaxPooling1D())
model4.add(Dropout(0.2))

model4.add(Dense(300))
model4.add(Dropout(0.2))
model4.add(BatchNormalization())


merged_model2 = Sequential()
merged_model2.add(Merge([model3, model4], mode='concat'))
merged_model2.add(BatchNormalization())

merged_model2.add(Dense(300))
merged_model2.add(PReLU())
merged_model2.add(Dropout(0.2))
merged_model2.add(BatchNormalization())

merged_model2.add(Dense(200))
merged_model2.add(PReLU())
merged_model2.add(Dropout(0.2))
merged_model2.add(BatchNormalization())

merged_model2.add(Dense(100))
merged_model2.add(PReLU())
merged_model2.add(Dropout(0.2))
merged_model2.add(BatchNormalization())

merged_model2.add(Dense(1))
merged_model2.add(Activation('sigmoid'))
```

### LSTM

```python
model5 = Sequential()
model5.add(Embedding(len(word_index) + 1,
                     300,
                     weights=[embedding_matrix],
                     input_length=40,
                     trainable=False))

model5.add(LSTM(300, dropout_W=0.2, dropout_U=0.2))

model6 = Sequential()
model6.add(Embedding(len(word_index) + 1,
                     300,
                     weights=[embedding_matrix],
                     input_length=40,
                     trainable=False))
model6.add(LSTM(300, dropout_W=0.2, dropout_U=0.2))

merged_model3 = Sequential()
merged_model3.add(Merge([model5, model6], mode='concat'))
merged_model3.add(BatchNormalization())

merged_model3.add(Dense(300))
merged_model3.add(PReLU())
merged_model3.add(Dropout(0.2))
merged_model3.add(BatchNormalization())

merged_model3.add(Dense(200))
merged_model3.add(PReLU())
merged_model3.add(Dropout(0.2))
merged_model3.add(BatchNormalization())

merged_model3.add(Dense(100))
merged_model3.add(PReLU())
merged_model3.add(Dropout(0.2))
merged_model3.add(BatchNormalization())

merged_model3.add(Dense(1))
merged_model3.add(Activation('sigmoid'))
```

### Bagging all the models

```python
model = Sequential()
model.add(Merge([merged_model, merged_model2, merged_model3], mode='concat'))
model.add(Dense(1))
model.add(Activation('sigmoid'))
```

### Compiling and training model

```python
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])

checkpoint = ModelCheckpoint('bagged.h5', monitor='val_acc', save_best_only=True, verbose=2)

history = model.fit([x1, x2, x1, x2, x1, x2], y=y, batch_size=350, nb_epoch=20,
                 verbose=1, validation_split=0.1, shuffle=True, callbacks=[checkpoint, metrics])
```

## Prediction

### Loading model

```python
model = load_model("model.h5")
```

### Taking input from user and saving in dataframe

```python
question1 = raw_input('question1: ')
question2 = raw_input('question2: ')
data = [[question1, question2]]
df = pd.DataFrame(data, columns=['question1','question2'])
```

### Padding the questions

```python
x1 = tk.texts_to_sequences(df.question1.values.astype(str))
x1 = sequence.pad_sequences(x1, maxlen=max_len)
x2 = tk.texts_to_sequences(df.question2.values.astype(str))
x2 = sequence.pad_sequences(x2, maxlen=max_len)
```

### Predicting

```python
pred = model.predict([x1,x2,x1,x2,x1,x2])

print pred
```
