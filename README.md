# project-diagnosticbreastcancer-googlecollabsimp
project-diagnosticbreastcancer-googlecollabsimp created by GitHub Classroom

# Cuprins
1. [Baza de date](#baza-de-date)
2. [Citirea datelor](#citirea-datelor)
3. [Simple CNN](#simple-cnn)
4. [VGG-16](#vgg16)
5. [DenseNet121](#densenet121)
6. [ResNet152](#resnet152)
7. [Bibliografie](#bibliografie)

# Introducere

Git-ul conține patru modele ale unor rețele neuronale convolutive folosite pentru a oferi un diagnostic pentru cancerul mamar. Cele patru rețele neuronale sunt implementate cu ajutorul librăriilor [Tensorflow](https://www.tensorflow.org/) și [Keras](https://keras.io/).

## Baza de date
Am ales [baza de date](http://www.eng.usf.edu/cvprg/Mammography/Database.html) publicata de universitatea din Florida. Dataset-ul nostru conține un training set și un test set. Fiecare dintre ele are trei subfoldere. Cele trei subfoldere reprezinta cele trei diagnostice: normal, malign, benign. Folderele conțin poze de tip jpg și png cu mamografiile din arhiva menționată anterior (am avut dificultăți în a citi formatul lor cu ImageGenerator și am decis să le citim ca png-uri).  
![logo4](https://user-images.githubusercontent.com/13523968/120293107-a0652700-c2cd-11eb-8e66-92e2b3348225.png)
![boobs](https://user-images.githubusercontent.com/13523968/120295200-9a704580-c2cf-11eb-9093-b4bac97a516b.PNG)

## Citirea datelor
Am citit datele ca fiind grayscale (cu excepția DenseNet unde arhitectura ne obligă să folosim culori). ImageGenerator creeaza un iterator pe întreg folder-ul dat.

## Simple CNN

E o arhitectură simplă creată de noi. Motivația unei astfel de arhitecturi este faptul că uneori un model mai simplu ca o regresie liniară sau o mașină cu suport vectorial poate da rezultate mult mai bune decât o arhitectură complexă. 

![simpleCNN](https://user-images.githubusercontent.com/79450729/120608341-c4ee0a00-c459-11eb-906a-63d8d947ca45.PNG)


## VGG16
VGG-16 este diferit de AlexNet prin faptul că înlocuiește filtrele de dimensiuni mari cu mai multe filtre cu nucleu de 3x3 unul după altul. Antrenarea lui durează foarte mult. Pentru [articolul în care a fost propus](https://arxiv.org/pdf/1409.1556.pdf), a fost antrenat mai multe săptămâni pe o placă video NVIDIA Titan Black.\
Inputul primului layer convolutiv are dimensiunea fixă de 224x224. Imaginea trece prin mai multe layere convolutive, cu filtre foarte mici, de 3x3 (cea mai mică dimensiune care poate captura noțiunea de sus/jos, stânga/dreapta și centru). Poolingul se face pe 5 layere de nucleu 2x2 care sunt așezate după un layer convolutiv (dar nu toate layerele convolutive sunt urmare de un layer de pooling).\
La finalul tuturor layerelor convolutive se află 3 straturi dense și, în sfârșit, layerul de softmax.\
Este de menționat faptul că fiecare layer ascuns este rectificat cu ReLU.

![vgg16](https://user-images.githubusercontent.com/13523968/120292122-b1f9ff00-c2cc-11eb-95fb-a7d5f4ab90a0.png)
![vgg16-neural-network](https://user-images.githubusercontent.com/13523968/120292197-c3dba200-c2cc-11eb-8b7a-f7184f5e3d51.jpg)

## DenseNet121
DenseNet-urile au nevoie de mai puțini parametri față de un CNN tradițional fiindcă nu e nevoie să învețe feature-uri reduntante.\
Acestea sunt foarte înguste fiindcă adaugă un set mic de feature map-uri noi.\
DenseNet-ul nu însumează output-urile feature map-urilor cu feature map-urile care vin de pe layerele anterioare, ci le concatenează.\
DenseNet-ul este separat în DenseBlock-uri unde dimensiunea feature map-urilor rămâne constantă întru-un bloc, dar numărul filtrelor se schimbă între ele. Acele layere sunt numite layere de tranziție și au grijă de downsampling aplicând batch normalization, o convoluție de nucleu 1x1 și un pooling layer de nucleu 2x2.

![1_B0urIaJjCIjFIz1MZOP5YQ](https://user-images.githubusercontent.com/13523968/120294929-59783100-c2cf-11eb-91ff-2acae2ecf9ea.png)
![1_GeK21UAbk4lEnNHhW_dgQA](https://user-images.githubusercontent.com/13523968/120294940-5bda8b00-c2cf-11eb-8cea-f8af514eeb0b.png)

## ResNet152
La fel ca motivația de la DenseNet, rețelele convolutive foarte adânci de obicei rezultă în risipirea gradientului atunci când este propagat înapoi pe layerele de început. ResNet folosește conceptul de blocuri reziduale, care include scurtături care te ajută să treci peste anumite layere.\
A înțelege un bloc rezidual este destul de ușor. În rețelele neuronale tradiționale, outputul layerelor se propagă în următorul layer. Într-o rețea cu blocuri reziduale, fiecare layer își propagă outputul în următorul layer și direct în layerele care sunt distanțate cu 2-3 pași mai în față. Mai multe detalii în [acest articol](https://arxiv.org/pdf/1512.03385.pdf).

![resnet](https://user-images.githubusercontent.com/13523968/121499116-c1bcc600-c9e5-11eb-87ac-ab27dddc77a6.jpg)

## Bibliografie
- [Residual Blocks](https://towardsdatascience.com/residual-blocks-building-blocks-of-resnet-fd90ca15d6ec)
- [CNN for Image Classifications](https://machinelearningmastery.com/review-of-architectural-innovations-for-convolutional-neural-networks-for-image-classification/)
- [Introduction to DenseNet with TensorFlow](https://www.pluralsight.com/guides/introduction-to-densenet-with-tensorflow)
- [Training a CNN to Detect Early Stage Lung Cancer Signs](https://www.youtube.com/watch?v=QZNF89cIje4)
- [CNN Architecture](https://www.youtube.com/watch?v=oV4YBitzXKw&t=571s)
- [Keras Documentation](https://keras.io/)
- [SuperDataScience](https://www.superdatascience.com/)
- [Why ResNets Work - Andrew Ng](https://www.youtube.com/watch?v=RYth6EbBUqM&start=466)
- [The ultimate guide to CNN](https://www.superdatascience.com/blogs/the-ultimate-guide-to-convolutional-neural-networks-cnn)
