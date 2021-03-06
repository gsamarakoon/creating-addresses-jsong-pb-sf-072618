
# Creating Addresses

#### Exercise Find the output address corresponding to this ScriptPubKey
```
76a914338c84849423992471bffb1a54a8d9b1d69dc28a88ac
```

Remember the structure of pay-to-pubkey-hash (p2pkh) which has `OP_DUP OP_HASH160 <hash> OP_EQUALVERIFY OP_CHECKSIG`.

You need to grab the hash160 and turn that into an address.


```python
from helper import h160_to_p2pkh_address
from script import Script

hex_script_pubkey = '76a914338c84849423992471bffb1a54a8d9b1d69dc28a88ac'

# bytes.fromhex to get binary
script_pubkey = bytes.fromhex(hex_script_pubkey)
# parse with Script
s = Script.parse(script_pubkey)
# get the 3rd element, which should be the hash160
h160 = s.elements[2]
# convert h160 to p2pkh address
print(h160_to_p2pkh_address(h160))
```

### Test Driven Exercise


```python
from script import Script
from helper import h160_to_p2sh_address

class Script(Script):

    def address(self, testnet=False):
        '''Returns the address corresponding to the script'''
        sig_type = self.type()
        if sig_type == 'p2pkh':
            # hash160 is the 3rd element
            h160 = self.elements[2]
            # convert to p2pkh address using h160_to_p2pkh_address (remember testnet)
            return h160_to_p2pkh_address(h160, testnet)
        elif sig_type == 'p2sh':
            # hash160 is the 2nd element
            h160 = self.elements[1]
            # convert to p2sh address using h160_to_p2sh_address (remember testnet)
            return h160_to_p2sh_address(h160, testnet)
```
