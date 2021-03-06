Code integration
================
To integrate FlowPrint into your own project, you can use it as a standalone module.
FlowPrint offers rich functionality that is easy to integrate into other projects.
Here we show some simple examples on how to use the FlowPrint package in your own python code.
For a complete documentation we refer to the :ref:`Reference` guide.

Import
^^^^^^
To import components from FlowPrint simply use the following format

.. code:: python

  from flowprint.<module> import <Object>

For example, the following code imports the :ref:`FlowPrint` and :ref:`Preprocessor` objects.

.. code:: python

  from flowprint.flowprint import FlowPrint
  from flowprint.preprocessor import Preprocessor

Flow extraction
^^^^^^^^^^^^^^^
To extract :ref:`Flow` objects from :code:`.pcap` files, we use the :ref:`Preprocessor` object.

.. code:: python

  # Imports
  from flowprint.preprocessor import Preprocessor

  # Create Preprocessor object
  preprocessor = Preprocessor(verbose=True)
  # Create Flows and labels
  X, y = preprocessor.process(files =['a.pcap', 'b.pcap'],
                              labels=['a', 'b'])

  # Save flows and labels to file 'flows.p'
  preprocessor.save('flows.p', X, y)
  # Load flows from file 'flows.p'
  X, y = preprocessor.load('flows.p')

Fingerprint generation
^^^^^^^^^^^^^^^^^^^^^^
To generate fingerprints we use the :ref:`FlowPrint` object.
We assume that the we have training flows and labels in variables :code:`X_train` and :code:`y_train` respectively, and have testing flows in variable :code:`X_test`.

.. code:: python

  # Imports
  from flowprint.flowprint import FlowPrint

  # Create FlowPrint object
  flowprint = FlowPrint(
      batch       = 300,
      window      = 30,
      correlation = 0.1,
      similarity  = 0.9
  )

  # Fit FlowPrint with flows and labels
  flowprint.fit(X_train, y_train)
  # Predict best matching fingerprints for each flow
  y_pred = flowprint.predict(X_test)

  # Store fingerprints to file 'fingerprints.json'
  flowprint.save('fingerprints.json')
  # Load fingerprints from file 'fingerprints.json'
  # This returns both the fingerprints and stores them in the flowprint object
  fingerprints = flowprint.load('fingerprints.json')

App recognition and detection
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
We can also use FlowPrint to recognize known apps or detect previously unseen apps.
Again, we assume that the we have training flows and labels in variables :code:`X_train` and :code:`y_train` respectively, and have testing flows in variable :code:`X_test`.

.. code:: python

  # Imports
  from flowprint.flowprint import FlowPrint

  # Create FlowPrint object
  flowprint = FlowPrint(
      batch       = 300,
      window      = 30,
      correlation = 0.1,
      similarity  = 0.9
  )

  # Fit FlowPrint with flows and labels
  flowprint.fit(X_train, y_train)

  # Recognise which app produced each flow
  y_recognize = flowprint.recognize(X_test)
  # Detect previously unseen apps
  # +1 if a flow belongs to a known app, -1 if a flow belongs to an unknown app
  y_detect    = flowprint.detect(X_test)
