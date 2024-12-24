.. _gen_data:

Generating Data
===============

Generally, there are two types of data a user may want to generated: data from a prespecified policy and data from a trained RL Agent. 
See :ref:`data_policies` and :ref:`data_rl` for more information. 

Data is generated over a range of ``years`` and range of ``latitudes`` and longitudes. These ranges can be specified by command line arguments:

.. code-block:: console

    python3 gen_data.py 
    --lat-low 50
    --lat-high 51
    --lon-low 120
    --lon-high 123
    --year-low 1984
    --year-high 2001

Additionally, two file types are supports ``.npz`` and ``.csv`` files. ``.npz`` files save the output in a 
``(obs, action, reward, next_obs, done, info)`` format which is compatible for Offline RL training and loading into Replay Buffers. Meanwhile, 
``.csv`` files save data in a daily observation format which can be useful for a variety of problems. 

To specify a file type, include the argument:

.. code-block:: console

    python3 gen_data.py 
    --file-type csv
    OR
    --file-type npz

.. note:: 

    As with any script in **WOFOST-Gym**, the ``--load-data-fpath`` argument is allowed, meaning that data can be generated from a previously created configuration file.


.. important::

    * Latitudes are valid in the range (-90, 90)
    * Longitudes are valid in the range (-180, 180)
    * Years are valid in the range (1984, 2023)
    * Outside of these ranges, a missing data error will be thrown from ``pcse.NASAPower``

.. _data_policies:

Prespecified Policies
---------------------

Many real-world agriculture policies are relatively simple and often open loop policies. That is, a valid policy may be `Apply 10 kg/ha of Nitrogen fertilizer every 3 weeks`.

One of the goals of the **WOFOST-Gym** benchmark is to support Off Policy Evaluation, and in many cases the baseline we are competing against is an open-loop policy, or a relatively simple closed-looop policy
such as `Apply 15 kg/ha of Nitrogen Fertilizer if the Nitrogen level in the soil drops below X amount`. 

To generate data from a prespecified policy, run the following:

.. code-block:: console

    python3 gen_data.py --file-type npz --save-folder test --data-file test_data --policy-name Below_N

This code block will generate data in ``.npz`` format, saving it to folder ``test`` under the name ``test_data.npz``. The generated data will be from the 
``Below_N`` policy located in ``pcse_gym/policies.py``. 

Every Prespecified policy takes three specific arguments: ``--threshold``, ``--interval``, and ``--amount``
which control the total amount that can be applied (or the threshold below which to take the action), the interval of application, and the amount to apply
of water, Nitrogen, Phosphorous, or Potassium. For more information on Prespecified Policies, see :ref:`policies`.

.. _data_rl:

RL Agent Policies
-----------------

For some use cases, we may want to use data generated from a trained RL agent to see how it performs in a variety of scenarios. 

To generate data from a trained RL Agent, run the following:

.. code-block:: console

    python3 gen_data.py --file-type npz --save-folder test --data-file test_data --agent-type <Agent Type> --agent_path <Path to agent.pt File>

This code block will generate data in ``.npz`` format, saving it to folder ``test`` under the name ``test_data.npz``. The generated data will be from the 
specified ``agent.pt`` file. 

.. important::
    
    * We make it very easy to specify RL agents, but this requires the ``RL_Agent.py`` file to be in a specific format. See :ref:`create_agent` for more information. 
    * Confirm that the ``agent_type`` and ``agent.pt`` file specified are of the same agent. 

    