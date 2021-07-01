# Design

+ [Min Stack](#min-stack)

+ [Implement Queue using Stacks](#implement-queue-using-stacks)

+ [Implement Stack using Queues](#implement-stack-using-queues)

+ [Design Twitter](#design-twitter)


## Min Stack

https://leetcode.com/problems/min-stack/

```python
class MinStack:
    def __init__(self):
        self.stack = []

    def push(self, val: int) -> None:
        if self.stack:
            if val <= self.stack[-1][1]:
                self.stack.append((val,val))
            else:
                self.stack.append((val,(self.stack[-1][1])))
        else:
            self.stack.append((val,val))
        
    def pop(self) -> None:
        self.stack.pop()

    def top(self) -> int:
        return self.stack[-1][0]

    def getMin(self) -> int:
        return self.stack[-1][1]
```

## Implement Queue using Stacks

https://leetcode.com/problems/implement-queue-using-stacks/

```python
class MyQueue:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.stack1 = []
        self.stack2 = []

    def push(self, x: int) -> None:
        """
        Push element x to the back of queue.
        """
        self.stack1.append(x)

    def pop(self) -> int:
        """
        Removes the element from in front of queue and returns that element.
        """
        try:
            out = self.stack2.pop()
        except:
            while self.stack1:
                self.stack2.append(self.stack1.pop())
            out = self.stack2.pop()                
        return out

    def peek(self) -> int:
        """
        Get the front element.
        """
        try:
            return self.stack2[-1]
        except:
            while self.stack1:
                self.stack2.append(self.stack1.pop())
            return self.stack2[-1]

    def empty(self) -> bool:
        """
        Returns whether the queue is empty.
        """
        if self.stack1 or self.stack2:
            return False
        else:
            return True
```


## Implement Stack using Queues

https://leetcode.com/problems/implement-stack-using-queues/

```python
class MyStack:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.queue1 = deque([])

    def push(self, x: int) -> None:
        """
        Push element x onto stack.
        """
        self.queue1.append(x)

    def pop(self) -> int:
        """
        Removes the element on top of the stack and returns that element.
        """
        return self.queue1.pop()

    def top(self) -> int:
        """
        Get the top element.
        """
        return self.queue1[-1] 

    def empty(self) -> bool:
        """
        Returns whether the stack is empty.
        """
        return not self.queue1
```


## Design Twitter

https://leetcode.com/problems/design-twitter/

```python
class Twitter:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.users = {}
        self.tweets = {}
        self.tweet_counter = -1

    def postTweet(self, userId: int, tweetId: int) -> None:
        """
        Compose a new tweet.
        """
        if userId not in self.users:
            self.users[userId] = set()
        if userId not in self.tweets:
            self.tweets[userId] = []
        list_of_tweets = self.tweets[userId]
        new_tweet = (self.tweet_counter, tweetId)
        list_of_tweets.append(new_tweet)
        self.tweets[userId] = list_of_tweets
        self.tweet_counter -= 1
        
    def getNewsFeed(self, userId: int) -> List[int]:
        """
        Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
        """
        heap = []
        if userId in self.tweets:
            my_tweets = self.tweets[userId]
            for tweet_number, tweet_Id in my_tweets:
                heapq.heappush(heap, (tweet_number, tweet_Id))
        if userId in self.users:
            my_followed = self.users[userId]
            for followed in my_followed:
                if followed in self.tweets:
                    their_tweets = self.tweets[followed]
                    for tweet_number, tweet_Id in their_tweets:
                        heapq.heappush(heap, (tweet_number, tweet_Id))
        return_list = []
        for i in range(len(heap)):
            if len(return_list) >= 10:
                break
            return_list.append(heapq.heappop(heap)[1])
        return return_list

    def follow(self, followerId: int, followeeId: int) -> None:
        """
        Follower follows a followee. If the operation is invalid, it should be a no-op.
        """
        if followerId == followeeId:
            return
        if followerId in self.users:
            followed = self.users[followerId]
            followed.add(followeeId)
            self.users[followerId] = followed
        else:
            self.users[followerId] = set()
            followed = self.users[followerId]
            followed.add(followeeId)
            self.users[followerId] = followed


    def unfollow(self, followerId: int, followeeId: int) -> None:
        """
        Follower unfollows a followee. If the operation is invalid, it should be a no-op.
        """
        if followerId in self.users:
            followed = self.users[followerId]
            if followeeId in followed:
                followed.remove(followeeId)
                self.users[followerId] = followed
        else:
            self.users[followerId] = set()

# Your Twitter object will be instantiated and called as such:
# obj = Twitter()
# obj.postTweet(userId,tweetId)
# param_2 = obj.getNewsFeed(userId)
# obj.follow(followerId,followeeId)
# obj.unfollow(followerId,followeeId)
```
