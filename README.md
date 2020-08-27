# IP-to-ASN mapping

## Prerequisites 

1. Install [`traceutils`](https://github.com/alexmarder/traceutils), which includes scripts to generate the IP-to-ASN mappings

1. Clone the [`retrieve-external`](https://github.com/alexmarder/retrieve-external) repository, which includs scripts to obtain routing datasets for the mapping of IPs to ASNs

1. Install the python libraries used by `retrieve-external`:

```
cd retrieve-external/
python3 -m pip install -r requirements.txt
```

## Generate the mapping of IP prefixes to ASNs

After you read the [documentation](https://alexmarder.github.io/retrieve-external/) of `retrieve-external` to understand the various options, execute the following steps:

1. Download the BGP data for a given range of dates and store them in a local directory. 
For example, the following command will download BGP RIBs for the period August 26-27 2020, and it will store them in the `ribs/` directory:

```
python3 retrieve.py -b 20200826 -e 20200827 -d ribs bgp
```

