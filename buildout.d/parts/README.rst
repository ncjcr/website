This directory contains buildouts that provide reusable buildout parts.

The idea is to have a self-contained components that can be reused in
different buildouts. Therefore all used variables, especially those
used in the corresponding templates files should be defined in this
configs. This can be checked by running a simple shell script::

  for x in buildout.d/templates/*
  do
    echo "--- $x";
    grep -o '\${[^}]*}' $x | sort -u;
  done
