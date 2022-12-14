We can practice this problem by clicking on this link.
>（ https://leetcode.com/problems/accounts-merge/ ）
# Description:
 <p> Given a list of accounts where each element accounts[i] is a list of strings, where the first element accounts[i][0] is a name, and the rest of the elements are emails representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails in sorted order. The accounts themselves can be returned in any order. </p> 

**example 1:**
```
Input: accounts = [["John","johnsmith@mail.com","john_newyork@mail.com"],["John","johnsmith@mail.com","john00@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
Output: [["John","john00@mail.com","john_newyork@mail.com","johnsmith@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
Explanation:
The first and second John's are the same person as they have the common email "johnsmith@mail.com".
The third John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], 
['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.
```

**example 2:**
```
Input: accounts = [["Gabe","Gabe0@m.co","Gabe3@m.co","Gabe1@m.co"],["Kevin","Kevin3@m.co","Kevin5@m.co","Kevin0@m.co"],["Ethan","Ethan5@m.co","Ethan4@m.co","Ethan0@m.co"],["Hanzo","Hanzo3@m.co","Hanzo1@m.co","Hanzo0@m.co"],["Fern","Fern5@m.co","Fern1@m.co","Fern0@m.co"]]
Output: [["Ethan","Ethan0@m.co","Ethan4@m.co","Ethan5@m.co"],["Gabe","Gabe0@m.co","Gabe1@m.co","Gabe3@m.co"],["Hanzo","Hanzo0@m.co","Hanzo1@m.co","Hanzo3@m.co"],["Kevin","Kevin0@m.co","Kevin3@m.co","Kevin5@m.co"],["Fern","Fern0@m.co","Fern1@m.co","Fern5@m.co"]]
```

**Constraints:**
```
1 <= accounts.length <= 1000
2 <= accounts[i].length <= 10
1 <= accounts[i][j].length <= 30
accounts[i][0] consists of English letters.
accounts[i][j] (for j > 0) is a valid email.
```

 ### Solution 1

```Python
graph = defaultdict(set)
        email_to_name = {}

        for account in accounts:
            name = account[0]
            email_one = account[1]
            email_to_name[email_one] = name

            for email in account[2:]:
                graph[email].add(email_one)
                graph[email_one].add(email)
                email_to_name[email] = name

        visited = set()
        results = []

        for email in email_to_name:
            if email not in visited:
                stack = [email]
                visited.add(email)
                emails = []
                while stack:
                    current_email = stack.pop()
                    emails.append(current_email)

                    for neighbor in graph[current_email]:
                        if neighbor not in visited:
                            stack.append(neighbor)
                            visited.add(neighbor)

                results.append([email_to_name[current_email]] + sorted(emails))

        return results
Time complexity: O(NlogN) Space complexity:O(N)
```
### Solution 2

```Python
class Solution:
    def accountsMerge(self, accounts: List[List[str]]) -> List[List[str]]:
        def find(x):
            if parents[x] != x:
                parents[x] = find(parents[x])

            return parents[x]

        def union(x, y):
            r_x = find(x)
            r_y = find(y)
            if r_x != r_y:
                parents[r_x] = r_y

        parents = {}
        email_to_name = {}

        for account in accounts:
            name = account[0]
            for email in account[1:]:
                parents[email] = email
                email_to_name[email] = name

        for account in accounts:
            email_one = account[1]
            for email in account[2:]:
                union(email, email_one)

        group = defaultdict(list)

        for email in parents:
            root = find(email)
            group[root].append(email)

        results = []

        for key in group:
            results.append([email_to_name[key]] + sorted(group[key]))
        return results



Time complexity: O(NlogN) Space complexity:O(N)
```
