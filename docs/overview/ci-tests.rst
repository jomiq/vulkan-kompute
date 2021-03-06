
CI, Docker Images & Tests
======================

This section contains an overview of the steps run on CI, as well as the tools used to simplify the testing (such as running Vulkan on CPU).

We use Github Actions to run the tests, which simplifies the workflows significantly for contributors. 

The tests run on CPU, and can be triggered using the ACT command line interface (https://github.com/nektos/act) - once you install the command line (And start the Docker daemon) you just have to type:

.. code-block::

    $ act

    [Python Tests/python-tests] 🚀  Start image=axsauze/kompute-builder:0.2
    [C++ Tests/cpp-tests      ] 🚀  Start image=axsauze/kompute-builder:0.2
    [C++ Tests/cpp-tests      ]   🐳  docker run image=axsauze/kompute-builder:0.2 entrypoint=["/usr/bin/tail" "-f" "/dev/null"] cmd=[]
    [Python Tests/python-tests]   🐳  docker run image=axsauze/kompute-builder:0.2 entrypoint=["/usr/bin/tail" "-f" "/dev/null"] cmd=[]
    ...


CI Commands Triggered
~~~~~~~~~~~~~

The simplest way to see how this works is by looking at the github actions commands that are run.

These can be found through the following files:

* `CPP Tests <https://github.com/EthicalML/vulkan-kompute/blob/master/.github/workflows/cpp_tests.yml>`_
* `Python Tests <https://github.com/EthicalML/vulkan-kompute/blob/master/.github/workflows/python_tests.yml>`_

When submitting a PR or merging a PR into master, both of these will run - you can see the logs through the github interface.



Running Vulkan on the CPU
~~~~~~~~~~~~~

We use `Swiftshader <https://github.com/google/swiftshader>`_ to enable us to run the Vulkan Kompute framework directly on the CPU for the CI tests.

Even though Swiftshader is optimized to function as a high-performance CPU backend for Vulkan, there are several limitations, the most notable in context of Kompute are:

* Loading files (spirv or text) leads to segfault
* Loading raw text string shaders leads to segfault

This is one of the main reason why only a subset of the tests are run in the CI.

Dockerfiles
~~~~~~~~~~~~~

The dockerfiles created provide functionality to simplify the interaction with the system. 

.. list-table::
   :header-rows: 1

   * - Image
     - Description
   * - axsauze/kompute-builder:0.2
     - Main CI builder image with all required dependencies to build and run C++ & Python tests.
   * - axsauze/swiftshader:0.1
     - Image building Swiftshader libraries only to reduce time via multi-staged builds
   * - axsauze/vulkan-sdk:0.1
     - Image contained a linux build of the full Vulkan SDK to reduce time via multi-staged builds



