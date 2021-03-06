Adding Regions and Institutions
===============================

Concept: Regions & Institutions
-------------------------------

|Cyclus| establishes a hierarchy of agents: Facility agents are operated by
Institution agents that exist within a Region agent.  This sense of ownership
and coarse "geolocation" allow for modifications of the interaction behavior
among agents.  For example, two facilities who trade in the same commodity,
but that exist in different regions may be disallowed from participating in a
trade.

Every |Cyclus| simulation needs at least one Region and one Institution, and
in this case, we'll use the simplest options:

* a Null Institution (*NullInst*) that holds a set of facilities that are
  deployed at the start of a simulation.
* a Null Region (*NullRegion*) that holds a set of Institutions.

Concept: Regions
----------------

Regions are the location of the facilties within a cyclus simulation.
Regions tie together a fuel cycle as they designate what facilities are
in the region's fuel cycle. Regions may apply preferences to each
potential request-bid pairing based on the proposed resource transfer.
The basic structure of a region is:

::

    <region>
      <name>Region_name</name>
      <config>
        <NullRegion/>
      </config>


    </region>

Where ``name`` is the name of the region. In between the two empty
spaces is where the institution and facility information goes. The
institution block is the form:

::

      <institution>
        <initialfacilitylist>
          <entry>
            <prototype>Prototype_name</prototype>
            <number>number_of_prototype_names</number>
          </entry>
          </initialfacilitylist>
        <name>Inst_name</name>
        <config>
          <NullInst/>
        </config>
      </institution>

Where ``prototype`` is the prototype that is in the region, ``number``
is the amount of prototypes in the institution, ``name`` is the name of
the institution. The final template is of the form:

::

    <region>
      <name>Region_name</name>
      <config>
        <NullRegion/>
      </config>
        <institution>
          <initialfacilitylist>
              <entry>
                <prototype>Prototype_name</prototype>
                <number>number_of_prototype_names</number>
              </entry>
          </initialfacilitylist>
        <name>Inst_name</name>
        <config>
          <NullInst/>
        </config>
       </institution>
    </region>

Concept: Institution - Institution agents (at least one required in each Region)
-----------------------------------------------------------------------
In *CYCLUS* input files, each institution block defines an agent that
acts as an institution in the simulation. An institution block can only
appear within a region block. Each institution block has the following
sections in any order:

-  ``name`` (required once) - a name for the prototype
-  ``lifetime`` (optional once) - a non-negative integer indicating the
   number of time steps that this region agent will be active in the
   simulation
-  ``config`` (required once) - the archetype-specific configuration
-  ``initialfacilitylist`` (optional, may appear multiple times) - a
   list of facility agents operating at the beginning of the simulation

Each ``initialfacilitylist`` block contains one or more ``entry`` blocks
that each contain the following sections, in the following order:

-  ``prototype`` - the name of a facility prototype defined elsewhere in
   the input file
-  ``number`` - the number of such facilities that are operating at the
   beginning of the simulation

The example below introduces two institution agents. This example
introduces two institution agents (the region section that encloses them
is not shown). The first institution has the name *SingleInstitution*,
and is configured from the archetype with the name (or alias)
*NullInst*. The author of the ``NullInst`` archetype has defined no
archetype-specific data. This agent begins the simulation with two
facility agents, one based on the ``FacilityA`` prototype and another
based on the ``FacilityB`` prototype. The second institution has the
name *AnotherInstitution*, is also configured from the archetype with
the name (or alias) ``NullInst``. This institution has no initial
facilities.

Activity: Write the Region template
+++++++++++++++++++++++++++++++++++

Using the table below, let's create the region section of our input file.


+----------------------+---------------------------+
| Variable             | Value                     |
+======================+===========================+
| ``name``             | ``USA``                   |
+----------------------+---------------------------+
| ``prototype``        | ``1178MWe BRAIDWOOD-1``   |
+----------------------+---------------------------+
| ``number``           | ``1``                     |
+----------------------+---------------------------+
| ``fuel_inrecipes``   | ``tails``                 |
+----------------------+---------------------------+

::

    <region>
      <name>[VALUE]</name>
      <config>
        <NullRegion/>
      </config>
      <institution>
        <initialfacilitylist>
          <entry>
            <prototype>[VALUE]</prototype>
            <number>[VALUE]</number>
          </entry>
          </initialfacilitylist>
        <name>[VALUE]</name>
        <config>
          <NullInst/>
        </config>
      </institution>

Now the next part of the region template is the other facilities in the
region's fuel cycle. In our example, these facilities are
``UraniumMine``, ``EnrichmentPlant``, and ``NuclearRepository``. Using
the above exercise and the table below, fill out the rest of the region
template.

+-----------------+-----------------------------+----------+
| Variable        | Name                        | Amount   |
+=================+=============================+==========+
| ``prototype``   | ``UraniumMine``             | ``1``    |
+-----------------+-----------------------------+----------+
| ``prototype``   | ``EnrichmentPlant``         | ``1``    |
+-----------------+-----------------------------+----------+
| ``prototype``   | ``NuclearRepository``       | ``1``    |
+-----------------+-----------------------------+----------+
| ``name``        | ``United States Nuclear``   | ``1``    |
+-----------------+-----------------------------+----------+

::

    <institution>
        <initialfacilitylist>
          <entry>
            <prototype>UraniumMine</prototype>
            <number>1</number>
          </entry>
          <entry>
            <prototype>EnrichmentPlant</prototype>
            <number>1</number>
          </entry>
          <entry>
            <prototype>NuclearRepository</prototype>
            <number>1</number>
          </entry>
        </initialfacilitylist>
        <name>United States Nuclear</name>
        <config>
          <NullInst/>
        </config>
      </institution>
    </region>


Activity: Save your input file
++++++++++++++++++++++++++++++

Save your input file as ``cyclus_intro_file.xml``

  - ``name`` is the name of the region
  - ``config`` is the archetype-specific configuration
  - ``institution`` - an institution agent operating in this region

Activity: Add a Region
++++++++++++++++++++++
Let's create region, ``USA``, that contains two institutions, ``Exelon`` and ``United States Nuclear``.
``Exelon`` is the institution that holds the ``1178MWe BRAIDWOOD-1`` reactor and ``United States Nuclear`` holds the ``UraniumMine``, ``EnrichmentPlant``, and ``NuclearRepository``.

.. image:: RIF_tutorial.png

Using the template above and the table below, let's build the region.

1. Since their are two institutions, 'Exelon' and ``United States Nuclear`` we will split the region into two parts.
Let's first build the ``Exelon`` institution. This institution has one ``1178MWe BRAIDWOOD-1`` prototype reactor. Using this information we can write this institution as:
::

  <region>
    <name>USA</name>
    <config>
      <NullRegion/>
    </config>
    <institution>
      <initialfacilitylist>
        <entry>
          <prototype>1178MWe BRAIDWOOD-1</prototype>
          <number>1</number>
        </entry>
        </initialfacilitylist>
      <name>Exelon </name>
      <config>
        <NullInst/>
      </config>
    </institution>

2. Now let's build the second institution, ``United States Nuclear``. This institution has one ``UraniumMine`` prototype, ``EnrichmentPlant`` prototype, and one ``NuclearRepository`` prototype. Using this information we can write this institution as:
::

    <institution>
        <initialfacilitylist>
          <entry>
            <prototype>UraniumMine</prototype>
            <number>1</number>
          </entry>
          <entry>
            <prototype>EnrichmentPlant</prototype>
            <number>1</number>
          </entry>
          <entry>
            <prototype>NuclearRepository</prototype>
            <number>1</number>
          </entry>
        </initialfacilitylist>
        <name>United States Nuclear</name>
        <config>
          <NullInst/>
        </config>
      </institution>

3. We will close the region section by appending the two sections together and appending a ``</region>`` tag to the end of the section. Once complete, your region prototype should look like:
::

  <region>
    <name>USA</name>
    <config>
      <NullRegion/>
    </config>
    <institution>
      <initialfacilitylist>
        <entry>
          <prototype>1178MWe BRAIDWOOD-1</prototype>
          <number>1</number>
        </entry>
        </initialfacilitylist>
      <name>Exelon</name>
      <config>
        <NullInst/>
      </config>
    </institution>

    <institution>
      <initialfacilitylist>
        <entry>
          <prototype>UraniumMine</prototype>
          <number>1</number>
        </entry>
        <entry>
          <prototype>EnrichmentPlant</prototype>
          <number>1</number>
        </entry>
        <entry>
          <prototype>NuclearRepository</prototype>
          <number>1</number>
        </entry>
      </initialfacilitylist>
      <name>United States Nuclear</name>
      <config>
        <NullInst/>
      </config>
    </institution>
  </region>

Activity: Generate (and Save) your Input File
+++++++++++++++++++++++++++++++++++++++++++++++

You are now ready to generate a full |Cyclus| input file.

1. Save your input file as 'cyclus_intro_file.xml'
