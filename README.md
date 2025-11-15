# Count-the-Number-of-Substrings-With-Dominant-Ones

You are given a binary string s.

Return the number of substrings with dominant ones.

A string has dominant ones if the number of ones in the string is greater than or equal to the square of the number of zeros in the string.

 class Solution(object):
    def numberOfSubstrings(self, s):
        """
        :type s: str
        :rtype: int
        """
        N = len(s)
        
        # next_zero[i] = index of closest zero after position i (or N if none exists)
        next_zero = [N] * N
        for i in range(N - 2, -1, -1):
            if s[i + 1] == "0":
                next_zero[i] = i + 1
            else:
                next_zero[i] = next_zero[i + 1]
        
        res = 0
        
        # For each starting position l
        for l in range(N):
            zeroes = 1 if s[l] == "0" else 0
            r = l
            
            # While we can still have valid substrings (zeroes² <= N)
            while zeroes * zeroes <= N:
                # next_z is the right boundary (next zero position or N)
                next_z = next_zero[r] if r < N else N
                
                # ones = number of ones from l to next_z-1
                ones = (next_z - l) - zeroes
                
                # If dominant: ones >= zeroes²
                if ones >= zeroes * zeroes:
                    # Count valid substrings
                    res += min(
                        next_z - r,
                        ones - zeroes * zeroes + 1
                    )
                
                # Move r to next zero
                r = next_z
                zeroes += 1
                
                # If we've reached the end, stop
                if r == N:
                    break
        
        return res
