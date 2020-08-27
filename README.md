# IP-to-ASN mapping

## Prerequisites 

1. Install [`traceutils`](https://github.com/alexmarder/traceutils), which includes scripts to parse routing data

1. Clone the [`retrieve-external`](https://github.com/alexmarder/retrieve-external) repository, which includs scripts to obtain routing datasets for the mapping of IPs to ASNs

1. Install the python libraries used by `retrieve-external`:

```
cd retrieve-external/
python3 -m pip install -r requirements.txt
```

1. Clone the [`ip2as`](https://github.com/alexmarder/ip2as) repository which includes scripts to generate the IP-to-ASN mapping based on the downloaded routing data.


## Generate the mapping of IP prefixes to ASNs

### Download the required routing data

After you read the [documentation](https://alexmarder.github.io/retrieve-external/) of `retrieve-external` to understand the various options, execute the following steps from inside the `retrieve-external` directory:

1. Download the BGP data for a given range of dates and store them in a local directory. 
For example, the following command will download BGP RIBs for the period August 26-27 2020, and it will store them in the `ribs/` directory.
Then generate a text file that lists the absolute paths of the downloaded files, one filepath per line:


```
python3 retrieve.py -b 20200826 -e 20200827 -d ribs bgp
ls -1d $PWD/ribs/* > ribs.20200827.txt
```

2. Download the extended RIR delegation files, which map the assigment of IP prefixes and ASNs to organizations based on RIR data and can be used to map prefixes to ASNs even if a prefix is not visible in the global routing table. 
Then generate a text file that lists the absolute paths of the downloaded files, one filepath per line:

```
python3 retrieve.py -b 20200826 -e 20200827 -d rirs rir
ls -1d $PWD/rirs/* > rirs.20200827.txt
```

3. Download the AS relationships, which can help to resolve inconsistencies in the BGP prefix advertisments

```
python3 retrieve.py -b 20200826 -e 20200827 -d rels rels
```

4. Download PeeringDB files which list the ASNs and IP prefixes are used by IXP peering LANs 

```
python3 retrieve.py -b 20200826 -e 20200827 -d ixps peeringdb
```

### Parse the downloaded routing data

After you read the [documentation](https://alexmarder.github.io/ip2as/) of `ip2as` to understand the various options, execute the following steps from inside the `ip2as` directory:

1. Extract Origin ASes from RIBs

```
 python3 prefixes.py -r ../retrieve-external/rirs.20200827.txt -p 4 -o ribs.prefixes
```
