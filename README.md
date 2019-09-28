# Largest-repeated-subset
Find the longest repeated subset of array elements in given array.
For example:
    for array('b','r','o','w','n','f','o','x','h','u','n','t','e','r','n','f','o','x','r','y','h','u','n')
    the longest repeated subset will be array('n','f','o','x').
 
The method should only accept an array of elements as param and return an array.

**Solution:

    function solve(array $list): array
    {
        $length = count($list);
        $longestRepeatedSequenceLength = intdiv($length + 1, 2);

        while ($longestRepeatedSequenceLength > 0) {
            $possibleIterations = intdiv($length, $longestRepeatedSequenceLength);
        
            for ($i = 0; $i < $possibleIterations; $i++) {
                $offset = $longestRepeatedSequenceLength + $i;

                if ($length - $offset < 0) {
                    continue;
                }

                $subset = array_slice($list, $i, $longestRepeatedSequenceLength);

                // take overlapping strings in consideration
                $testIn = ($longestRepeatedSequenceLength > 1) ?
                    array_slice($list, $offset - 1, $length - $offset + 1) :
                    array_slice($list, $offset, $length - $offset);

                if (isSubset($testIn, $subset)) {
                    return $subset;
                }
            }
        
            $longestRepeatedSequenceLength--;
        }

        return [];
    }
    
    /**
     * Check if $subArray is a subset of $arr.
     *
     * @param array $arr      : The main array
     * @param array $subArray : The sub-array
     *
     * @return array bool
     */
    function isSubset(array $arr, array $subArray): bool
    {
        if (count($arr) < count($subArray)) {
            return false;
        }
        
        $keys = array_keys($arr, $subArray[array_keys($subArray)[0]]);
    
        foreach ($keys as $k) {
            if (array_slice($arr, $k, count($subArray)) === $subArray) {
                return true;
            }
        }
    
        return false;
    }
    
**And some tests:

// here are some test-cases that your solution will be tested against, add more

assert(solve([]) === []);
assert(solve(['aa', 'a', 'a']) === ['a']);
assert(solve(['a', 'b', 'c', 'a', 'b', 'c', 'a']) === ['a', 'b', 'c', 'a']);
assert(solve(['a', 'b', 'c']) === []);
assert(solve(['b','r','o','w','n','f','o','x','h','u','n','t','e','r','n','f','o','x','r','y','h','u','n']) === ['n','f','o','x']);

// Interface should not be modified
assert(function_exists('solve'));
assert((new ReflectionFunction('solve'))->getParameters()[0]->isArray());
// another simple case
assert(solve(['b','a','n','a','n','a']) === ['a','n','a']);
// type should match
assert(solve([0, 1, 0, '1', 0, 1, 0, 1, 0, 1]) === [0, 1, 0]);
// it should not care about what is in the list
assert(solve(['a', 'b', 'c', 'ab', 'c', 'a', 'b', 'c']) === ['a', 'b', 'c']);

assert(solve([false, null, 0, false, null, '0']) === [false, null]);

$a = new StdClass();

assert(solve([$a, $a, $a]) === [$a, $a]);

$b = clone $a;

assert(solve([$a, $b, $a, $b]) === [$a, $b]);

$c = function($a) {};

$d = function($b) {};

assert(solve([$c, $d, $c, $d]) === [$c, $d]);

assert(solve([[],[], []]) === [[], []]);
