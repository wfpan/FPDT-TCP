# FPDT-TCP
To compare with the method proposed in Reference [14], our FPDT-TCP is developed based on the source code provided in Reference [14]. We embedded our self-designed code into the original source code from Reference [14].

Our approach can be executed via the class it.unina.dieti.tkprio.DeterministicTechniques, and it is instantiated by the statement: new QuotientSetIDFPrioritization3(new MTKPrioritizationWithMethodCognitiveIDF2())

To run our method, researchers may import the project into Eclipse and configure the corresponding dependency packages.

The execution results of the program will be stored in an SQLite database. The class it.unina.dieti.tkprio.ExportCSVResults can be used to export these results into a CSV file.

[14] F. Altiero, A. Corazza, S. Di Martino, A. Peron, and L. Libero Lucio Starace, “Regression test prioritization leveraging source code similarity with tree kernels,” J. Softw.: Evol. Process, vol. 36, no. 8,
2024.
