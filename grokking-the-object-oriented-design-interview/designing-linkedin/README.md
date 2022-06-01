# Designing LinkedIn

LinkedIn is a social network for professionals. The main goal of the site is to enable its members to connect with people they know and trust professionally, as well as to find new opportunities to grow their careers.

A LinkedIn member’s profile page, which emphasizes their skills, employment history, and education, has professional network news feeds with customizable modules.

LinkedIn is very similar to Facebook in terms of its layout and design. These features are more specialized because they cater to professionals, but in general, if you know how to use Facebook or any other similar social network, LinkedIn is somewhat comparable.

## Requirements

- Each member should be able to add information about their basic profile, experiences, education, skills, and accomplishments.
- Any user of our system should be able to search for other members or companies by their name.
- Members should be able to send or accept connection requests from other members.
- Any member will be able to request a recommendation from other members.
- The system should be able to show basic stats about a profile, like the number of profile views, the total number of connections, and the total number of search appearances of the profile.
- Members should be able to create new posts to share with their connections.
- Members should be able to add comments to posts, as well as like or share a post or comment.
- Any member should be able to send messages to other members.
- The system should send a notification to a member whenever there is a new message, connection invitation or a comment on their post.
- Members will be able to create a page for a Company and add job postings.
- Members should be able to create groups and join any group they like.
- Members should be able to follow other members or companies.

## Use Case Diagram

### Actors

- Member: All members can search for other members, companies or jobs, as well as send requests for connection, create posts, etc.
- Admin: Mainly responsible for admin functions such as blocking and unblocking a member, etc.
- System: Mainly responsible for sending notifications for new messages, connections invites, etc.

### Processes

- Add/update profile: Any member should be able to create their profile to reflect their experiences, education, skills, and accomplishments.
- Search: Members can search other members, companies or jobs. Members can send a connection request to other members.
- Follow or Unfollow member or company: Any member can follow or unfollow any other member or a company.
- Send message: Any member can send a message to any of their connections.
- Create post: Any member can create a post to share with their connections, as well as like other posts or add comments to any post.
- Send notifications: The system will be able to send notifications for new messages, connection invites, etc.

![Use Case Diagram of LinkedIn](use-case-diagram-of-linkedin.svg)

## Class Diagram

- Member: This will be the main component of our system. Each member will have a profile which includes their Experiences, Education, Skills, Accomplishments, and Recommendations. Members will be connected to other members and they can follow companies and members. Members will also have suggestions to make connections with other members.
- Search: Our system will support searching for other members and companies by their names, and jobs by their titles.
- Message: Members can send messages to other members with text and media.
- Post: Members can create posts containing text and media.
- Comment: Members can add comments to posts as well as like them.
- Group: Members can create and join groups.
- Company: Company will store all the information about a company’s page.
- JobPosting: Companies can create a job posting. This class will handle all information about a job.
- Notification: Will take care of sending notifications to members.

![Class Diagram of LinkedIn](class-diagram-of-linkedin.png)

![UML Conventions](uml-conventions.svg)

## Activity Diagram

### Add Experience

![Activity Diagram of LinkedIn - Add Experience](activity-diagram-of-linkedin-add-experience.svg)

### Send Message

![Activity Diagram of LinkedIn - Send Message](activity-diagram-of-linkedin-send-message.svg)

## Code

### Constants and Enums

```java
public enum ConnectionInvitationStatus {
  PENDING, ACCEPTED, CONFIRMED, REJECTED, CANCELED
}

public enum AccountStatus {
  ACTIVE, BLOCKED, BANNED, COMPROMISED, ARCHIVED, UNKNOWN
}

public class Address {
  private String streetAddress;
  private String city;
  private String state;
  private String zipCode;
  private String country;
}
```

### Account, Person, Member, and Admin

```java
// For simplicity, we are not defining getter and setter functions. The reader can
// assume that all class attributes are private and accessed through their respective
// public getter method and modified only through their public setter method.

public class Account {
  private String id;
  private String password;
  private AccountStatus status;

  public boolean resetPassword();
}

public abstract class Person {
  private String name;
  private Address address;
  private String email;
  private String phone;

  private Account account;
}

public class Member extends Person {
  private Date dateOfMembership;
  private String headline;
  private byte[] photo;
  private List<Member> memberSuggestions;
  private List<Member> memberFollows;
  private List<Member> memberConnections;
  private List<Company> companyFollows;
  private List<Group> groupFollows;
  private Profile profile;

  public boolean sendMessage(Message message);
  public boolean createPost(Post post);
  public boolean sendConnectionInvitation(ConnectionInvitation invitation);
}

public class Admin extends Person {
  public boolean blockUser(Customer customer);
  public boolean unblockUser(Customer customer);
}
```

### Profile, Experience, etc.

```java
public class Profile {
  private String summary;
  private List<Experience> experiences;
  private List<Education> educations;
  private List<Skill> skills;
  private List<Accomplishment> accomplishments;
  private List<Recommendation> recommendations;
  private List<Stat> stats;

  public boolean addExperience(Experience experience);
  public boolean addEducation(Education education);
  public boolean addSkill(Skill skill);
  public boolean addAccomplishment(Accomplishment accomplishment);
  public boolean addRecommendation(Recommendation recommendation);
}

public class Experience {
  private String title;
  private String company;
  private String location;
  private Date from;
  private Date to;
  private String description;
}
```

### Company and JobPosting

```java
public class Company {
  private String name;
  private String description;
  private String type;
  private int companySize;

  private List<JobPosting> activeJobPostings;
}

public class JobPosting {
  private Date dateOfPosting;
  private String description;
  private String employmentType;
  private String location;
  private boolean isFulfilled;
}
```

### Group, Post, and Message

```java
public class Group {
  private String name;
  private String description;
  private int totalMembers;
  private List<Member> members;

  public boolean addMember(Member member);

  public boolean updateDescription(String description);
}

public class Post {
  private String text;
  private int totalLikes;
  private int totalShares;
  private Member owner;
}

public class Message {
  private Member[] sentTo;
  private String messageBody;
  private byte[] media;
}
```

### Search Interface and SearchIndex

```java
public interface Search {
  public List<Member> searchMember(String name);

  public List<Company> searchCompany(String name);

  public List<JobPosting> searchJob(String title);
}

public class SearchIndex implements Search {
  HashMap<String, List<Member>> memberNames;
  HashMap<String, List<Company>> companyNames;
  HashMap<String, List<JobPosting>> jobTitles;

  public boolean addMember(Member member) {
    if (memberNames.containsKey(member.getName())) {
      memberNames.get(member.getName()).add(member);
    } else {
      memberNames.put(member.getName(), member);
    }
  }

  public boolean addCompany(Company company);

  public boolean addJobPosting(JobPosting jobPosting);

  public List<Member> searchMember(String name) {
    return memberNames.get(name);
  }

  public List<Company> searchCompany(String name) {
    return companyNames.get(name);
  }

  public List<JobPosting> searchJob(String title) {
    return jobTitles.get(title);
  }
}
```
