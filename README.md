
import java.util.Arrays;

class Solution {
    public int findRadius(int[] houses, int[] heaters) {

        Arrays.sort(heaters);

        int ans = 0;

        for (int house : houses) {

            int idx = Arrays.binarySearch(heaters, house);

            if (idx >= 0) {
                continue;
            }

            idx = -idx - 1;

            int left = (idx > 0)
                ? house - heaters[idx - 1]
                : Integer.MAX_VALUE;

            int right = (idx < heaters.length)
                ? heaters[idx] - house
                : Integer.MAX_VALUE;

            int minDistance = Math.min(left, right);

            ans = Math.max(ans, minDistance);
        }

        return ans;
    }
}
