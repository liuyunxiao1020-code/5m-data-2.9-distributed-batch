# Lesson

## Brief

### Preparation and Lesson Overview

We will be using Google Colaboratory (you can use the same google account created in the previous unit) in this lesson. Google Colaboratory (Colab) is a free Jupyter notebook environment that requires no setup and runs entirely in the cloud and can be stored in your Google Drive.

Colab runs in a Linux environment and comes with many popular Python libraries pre-installed. You can also install additional libraries using `pip` commands. It is a great tool for learning and experimenting with data science and machine learning. Read more about it in [Pre-Class](./pre-class.md).

We will be running Spark on Colab. As Spark is a distributed data processing framework written in Scala and runs on the Java Virtual Machine (JVM), it requires a Java runtime environment to run. Colab comes with a Java runtime environment pre-installed, so we can run Spark on Colab without any additional setup.

---

## Part 1 - Big Data Ecosystem and Batch Processing

Conceptual knowledge, refer to slides.

---

## Part 2 - Hands-on with Spark

We will be using [this notebook](https://colab.research.google.com/drive/1vmXtzmqDxjs_d-u8cZWtCZnZpNtMcQJx) file and connect to Colab over web browser.

> Open the Colab link, then go to File -> Save a copy in Drive. This will create a copy of the notebook in your Google Drive. It will then open the notebook in a new tab.
>
> Follow on with the lesson in the notebook.


Alternatively, we can use the notebook [5m_data_2_9_Hands_on_with_Spark_(4).ipynb](notebook/5m_data_2_9_Hands_on_with_Spark_(4).ipynb) in the `notebook` folder. However, we will be connecting this notebook to the Colab environment using the Colab extension. 

- Open the notebook `m_data_2_9_Hands_on_with_Spark.ipynb` under the notebook folder.
- Click n the top right corner `Select kernel` and then select `Colab` instead. It will direct you to a browser to login to Google.
- After login to Google, come back to the notebook and select `Python 3`

> Please note that `psdf.plot()` will not show plot in Colab in VSCode.
