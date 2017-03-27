# What I learned about Class Inheritance vs Prototype Inheritance

The biggest thing to take away is that 
> The solution to all of these problems is to favor object composition over class inheritance.

> You're out of your element Class Inheritance!!

1. Class Inheritance
   1. Example
   1. Why it is a bad choice

2. Prototype Inheritance
   2. Example
   2. Why it is a better choice


## Class Inheritance

### Example

```javascript
class GuitarAmp {
  constructor ({ cabinet = 'spruce', distortion = '1', volume = '0' } = {}) {
    Object.assign(this, {
      cabinet, distortion, volume
    });
  }
}

class BassAmp extends GuitarAmp {
  constructor (options = {}) {
    super(options);
    this.lowCut = options.lowCut;
  }
}

class ChannelStrip extends BassAmp {
  constructor (options = {}) {
    super(options);
    this.inputLevel = options.inputLevel;
  }
}

test('Class Inheritance', nest => {
  nest.test('BassAmp', assert => {
    const msg = `instance should inherit props
    from GuitarAmp and BassAmp`;

    const myAmp = new BassAmp();
    const actual = Object.keys(myAmp);
    const expected = ['cabinet', 'distortion', 'volume', 'lowCut'];

    assert.deepEqual(actual, expected, msg);
    assert.end();
  });

  nest.test('ChannelStrip', assert => {
    const msg = 'instance should inherit from GuitarAmp, BassAmp, and ChannelStrip';
    const myStrip = new ChannelStrip();
    const actual = Object.keys(myStrip);
    const expected = ['cabinet', 'distortion', 'volume', 'lowCut', 'inputLevel'];

    assert.deepEqual(actual, expected, msg);
    assert.end();
  });
});
```

### Why it is a bad choice

As you can see when going down through the code each class extends the other. <strong>BassAmp</strong> extends <strong>GuitarAmp</strong>, <strong>ChannelStrip</strong> extends <strong>BassAmp</strong> and it would continue this way until it was finished. If you had to change something within <strong>ChannelStrip</strong> you would have to alter the other classes. This is not effiecent and you will be spending more time writing , re-writing, writing, and re-writing code than you should have to. 


## Prototype Inheritance
     
### Example

```javascript
const distortion = { distortion: 1 };
const volume = { volume: 1 };
const cabinet = { cabinet: 'maple' };
const lowCut = { lowCut: 1 };
const inputLevel = { inputLevel: 1 };

const GuitarAmp = (options) => {
  return Object.assign({}, distortion, volume, cabinet, options);
};

const BassAmp = (options) => {
  return Object.assign({}, lowCut, volume, cabinet, options);
};

const ChannelStrip = (options) => {
  return Object.assign({}, inputLevel, lowCut, volume, options);
};


test('GuitarAmp', assert => {
  const msg = 'should have distortion, volume, and cabinet';
  const level = 2;
  const cabinet = 'vintage';

  const actual = GuitarAmp({
    distortion: level,
    volume: level,
    cabinet
  });
  const expected = {
    distortion: level,
    volume: level,
    cabinet
  };

  assert.deepEqual(actual, expected, msg);
  assert.end();
});

test('BassAmp', assert => {
  const msg = 'should have volume, lowCut, and cabinet';
  const level = 2;
  const cabinet = 'vintage';

  const actual = BassAmp({
    lowCut: level,
    volume: level,
    cabinet
  });
  const expected = {
    lowCut: level,
    volume: level,
    cabinet
  };

  assert.deepEqual(actual, expected, msg);
  assert.end();
});

test('ChannelStrip', assert => {
  const msg = 'should have inputLevel, lowCut, and volume';
  const level = 2;

  const actual = ChannelStrip({
    inputLevel: level,
    lowCut: level,
    volume: level
  });
  const expected = {
    inputLevel: level,
    lowCut: level,
    volume: level
  };

  assert.deepEqual(actual, expected, msg);
  assert.end();
});
```

### Why it is a better choice

This code just looks cleaner. It starts off with your variables being declared so that they can be used later. But the beauty is that you name all of them here and just plug which ones you need into <strong>GuitarAmp</strong>, <strong>BassAmp</strong> and <strong>ChannelStrip</strong>
without having to alter the other. So in the future when you have to add more all you have to do is add some more variables and enter the new const. It will take a considerable less amount of time to add in the future and you don't run the risk of messing anything up since you are only adding and not altering. 

http://codepen.io/Failure2Fly/pen/MpgPjY