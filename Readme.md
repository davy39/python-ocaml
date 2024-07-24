# Build of ocaml python package for using with jupyter


```bash
git clone https://github.com/janestreet/pythonlib
git checkout v0.15
cd pythonlib
opam init -c 4.11.0
eval $(opam env --switch=4.11.0)
opam install . --deps-only
opam install ppx_jane

LIBS="
  base base/base_internalhash_types
  ppx_expect/collector time_now bin_prot pyml stdcompat"

OPAM_LIBS="$HOME/.opam/4.11.0/lib"

for l in $LIBS
do
  export LIBRARY_PATH="$OPAM_LIBS/$l:$LIBRARY_PATH"
done

# Modify pythonlib/examples/init/dune with your python version

dune build examples/ocaml.bc.so

cd examples

mkdir sharedlib
cp _build/default/examples/ocaml.bc.so sharedlib/ocaml.so
cp ~/.opam/4.11.0/lib/ocaml/stdlib.cmi sharedlib/stdlib.cmi

python setup.py sdist bdist_wheel

pip install dist/ocaml-0.0.5-py3-none-any.whl
```
