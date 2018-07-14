
## Is saving and loading arrays faster in NumPy or PyTorch?

Test performed on 2018-07-14

```
import torch 
import numpy as np

# speed test: PyTorch (v0.4.0) vs Numpy (v1.14.2)

## PyTorch

rnd = torch.rand(10000,10000)

%time torch.save( rnd , 'testcsv2' )
#New file:  Wall time: 156 ms
#Overwrite: Wall time: 2.21 s

%time tmp = torch.load('testcsv2');
# Wall time: 177 ms

## Numpy

rnd_numpy = rnd.numpy()

%time np.save('testcsv2.np', rnd_numpy)

#New file:  Wall time: 189 ms
#Overwrite: Wall time: 2.07 s


%time tmp = np.load('testcsv2.np.npy')
# Wall time: 184 ms
```

**Conclusion** both have similar performance. 

## With compression

```
%time np.savez_compressed('testcsv3.np', rnd_numpy)
#New file:  Wall time: 14.2 s

%time tmp = np.load('testcsv3.np.npz').items()
# Wall time: 1.92 s
```

file size was reduced from 380MB to ~340MB. Intrestingly, but __the loading performance is order of magnitude worse__. 
