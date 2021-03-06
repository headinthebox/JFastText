## Introduction
JFastText is a Java wrapper for Facebook's [fastText](https://github.com/facebookresearch/fastText), 
a library for efficient learning of word embeddings and fast sentence classification. The Java interface
is built using [javacpp](https://github.com/bytedeco/javacpp). 

This library provides full fastText's command line interface. It also provides the API for 
loading trained model from file to do label prediction in memory. Model training is only done 
via the command line interface. Hence, JFastText is ideal for building Java Web applications 
for text classification using the model trained offline.

## Maven dependency
```xml
<dependency>
  <groupId>com.github.vinhkhuc</groupId>
  <artifactId>jfasttext</artifactId>
  <version>0.1</version>
</dependency>
```


## Building
Maven 3.x and C++ compiler (g++ on Mac/Linux or cl.exe on Windows) are required to build the Jar package. 

```bash
git clone --recursive https://github.com/vinhkhuc/JFastText
cd JFastText
mvn clean package
```

## Examples
Examples on how to use JFastText's API and its command line interface can be found in the folders
[examples/api](examples/api) and [examples/cmd](examples/cmd).

## How to use

### Initialization

```java
import com.github.jfasttext.JFastText;
JFastText jft = new JFastText();
```

### Text classification
```java
// Train supervised model
jft.runCmd(new String[] {
        "supervised",
        "-input", "src/test/resources/data/labeled_data.txt",
        "-output", "src/test/resources/models/supervised.model"
});

// Load model from file
jft.loadModel("src/test/resources/models/supervised.model.bin");

// Do label prediction
String text = "What is the most popular game in the US ?";
JFastText.ProbLabel probLabel = jft.predictProba(text);
System.out.printf("\nThe label of '%s' is '%s' with probability %f\n",
        text, probLabel.label, Math.exp(probLabel.logProb));
``` 
 
### Word embedding learning
```java
jft.runCmd(new String[] {
        "skipgram",
        "-input", "src/test/resources/data/unlabeled_data.txt",
        "-output", "src/test/resources/models/skipgram.model",
        "-bucket", "100",
        "-minCount", "1"
});
```

### FastText's command line
```bash
$ java -jar target/jfasttext-*-jar-with-dependencies.jar
usage: fasttext <command> <args>

The commands supported by fasttext are:

  supervised          train a supervised classifier
  test                evaluate a supervised classifier
  predict             predict most likely labels
  predict-prob        predict most likely labels with probabilities
  skipgram            train a skipgram model
  cbow                train a cbow model
  print-vectors       print vectors given a trained model
```

## License
BSD

## References
(From fastText's [references](https://github.com/facebookresearch/fastText#references))

Please cite [1](#enriching-word-vectors-with-subword-information) if using this code for learning word representations or [2](#bag-of-tricks-for-efficient-text-classification) if using for text classification.

### Enriching Word Vectors with Subword Information

[1] P. Bojanowski\*, E. Grave\*, A. Joulin, T. Mikolov, [*Enriching Word Vectors with Subword Information*](https://arxiv.org/abs/1607.04606)

```
@article{bojanowski2016enriching,
  title={Enriching Word Vectors with Subword Information},
  author={Bojanowski, Piotr and Grave, Edouard and Joulin, Armand and Mikolov, Tomas},
  journal={arXiv preprint arXiv:1607.04606},
  year={2016}
}
```

### Bag of Tricks for Efficient Text Classification

[2] A. Joulin, E. Grave, P. Bojanowski, T. Mikolov, [*Bag of Tricks for Efficient Text Classification*](https://arxiv.org/abs/1607.01759)

```
@article{joulin2016bag,
  title={Bag of Tricks for Efficient Text Classification},
  author={Joulin, Armand and Grave, Edouard and Bojanowski, Piotr and Mikolov, Tomas},
  journal={arXiv preprint arXiv:1607.01759},
  year={2016}
}
```

(\* These authors contributed equally.)

