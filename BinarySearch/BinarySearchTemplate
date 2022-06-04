#include <iostream>
#include <vector>
using namespace std;
int search(vector<int> &nums, int target)
{
    int l = 0, m, r = nums.size();
    while (l < r)
    {
        m = (l + r) >> 1;
        if (nums[m] < target)
            l = m + 1;
        if (nums[m] == target)
            return m;
        if (nums[m] > target)
            r = m;

    }
    return -1;
}
int main()
{
    vector<int> v = {-1,0,3,5,9,12};
    search(v, 9);
}
