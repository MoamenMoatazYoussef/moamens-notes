import java.util.HashMap;

public class myTuple<X, Y>
{
    public X x;
    public Y y;
    public myTuple<X, Y>(X x, Y y)
    {
        this.x = x;
        this.y = y;
    }
}

public class Solution {
    /*public class myTuple<X, Y>
    {
        public X x;
        public Y y;
        public myTuple(X x, Y y)
        {
            this.x = x;
            this.y = y;
        }
    }*/
    
    static void testMajorityElement()
    {
        int a1[] = {3, 2, 3};
        System.out.println(majorityElement(a1));
        
        int a2[] = {3, 4, 3, 4, 3, 9, 3, 1, 3};
        System.out.println(majorityElement(a2));
                           
        int a3[] = {3, 4, 3, 4, 3, 4, 3, 3};
        System.out.println(majorityElement(a3));
        
        int a4[] = {1};
        System.out.println(majorityElement(a4));
    }
    
    static myTuple<int, int> majorityElementAux(int[] nums, int start, int end)
    {
        int l = nums.length;
        if(l >= 3)
        {
            int firstPart  = start + (end - start)/3 + 1;
            int secondPart = start + ((end - start)/3) * 2) + 1;
            myTuple<int, int> a = majorityElementAux(nums, start, firstPart);
            myTuple<int, int> b = majorityElementAux(nums, firstPart + 1, secondPart);
            myTuple<int, int> c = majorityElementAux(nums, secondPart + 1, end);
            array of a.y, b.y, c.y
            get max
            get index of max
            get max tuple
            return max tuple;
        }
        make hashmap
        fill hashmap with num[] as key, # as value
        loop:
            if(hashmap at key has value > 1)
                max = that key
            else
                max = first key
        tuple<> = (maxKey, maxValue)
        return tuple;
    }
    
    static int majorityElement(int[] nums) {
        return majorityElementAux(nums, 0, nums.length);
    }
    
    public static void main(String[] args)
    {
        testMajorityElement();
    }
}