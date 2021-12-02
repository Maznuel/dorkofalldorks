# dorkofalldorks
using google dork for carding
<?php

function topinfo()
{
    include '../init.php';
    if (mysqli_connect_errno()) {
        echo "Failed to connect to MySQL: " . mysqli_connect_error();
    }
    $sql = "SELECT * FROM cards WHERE Top=1";
    $result = mysqli_query($conn, $sql);
    // This line executes the MySQL query that you typed above
    $toparray = array();
    // make a new array to hold all your data
    $index = 0;
    while ($row = mysqli_fetch_assoc($result)) {
        // loop to store the data in an associative array.
        $toparray[$index] = $row;
        $index++;
    }
    echo json_encode($toparray);
    include '../close.php';
}
function newinfo()
{
    include '../init.php';
    $conn->query("SET NAMES utf8");
    if (mysqli_connect_errno()) {
        echo "Failed to connect to MySQL: " . mysqli_connect_error();
    }
    $sql = "SELECT * FROM cards WHERE New=1";
    $result = mysqli_query($conn, $sql);
    // This line executes the MySQL query that you typed above
    $toparray = array();
    // make a new array to hold all your data
    $index = 0;
    while ($row = mysqli_fetch_assoc($result)) {
        // loop to store the data in an associative array.
        $toparray[$index] = $row;
        $index++;
    }
    echo json_encode($toparray);
    include '../close.php';
}
function cardinfo()
{
    if (isset($_GET['id'])) {
        include '../init.php';
        if (mysqli_connect_errno()) {
            echo "Failed to connect to MySQL: " . mysqli_connect_error();
        }
        $conn->query("SET NAMES utf8");
        $tid = $_GET['id'];
        $_GET['Title'] = $conn->query("SELECT Title FROM cards WHERE id = {$tid}")->fetch_object()->Title;
        $_GET['Discount'] = $conn->query("SELECT Discount FROM cards WHERE id = {$tid}")->fetch_object()->Discount;
        $_GET['CoverPhoto'] = $conn->query("SELECT CoverPhoto FROM cards WHERE id = {$tid}")->fetch_object()->CoverPhoto;
        $_GET['ShortDescription'] = $conn->query("SELECT ShortDescription FROM cards WHERE id = {$tid}")->fetch_object()->ShortDescription;
        $_GET['LongDescription'] = $conn->query("SELECT LongDescription FROM cards WHERE id = {$tid}")->fetch_object()->LongDescription;
        $_GET['PassedDays'] = $conn->query("SELECT PassedDays FROM cards WHERE id = {$tid}")->fetch_object()->PassedDays;
        $_GET['LeftDays'] = $conn->query("SELECT LeftDays FROM cards WHERE id = {$tid}")->fetch_object()->LeftDays;
        $SQLCommand = "SELECT photos.Photo\n\t\t\t\t\tFROM cards INNER JOIN photos ON cards.ID = photos.CardID\n\t\t\t\t\tWHERE cards.ID={$tid}";
        $result = mysqli_query($conn, $SQLCommand);
        // This line executes the MySQL query that you typed above
        $_GET['Logo'] = $conn->query("SELECT partners.Logo\n\t\t\t\t\tFROM cards INNER JOIN partners ON cards.PartnerID = partners.ID\n\t\t\t\t\tWHERE cards.ID={$tid}")->fetch_object()->Logo;
        $_GET['Tel'] = $conn->query("SELECT partners.Tel\n\t\t\t\t\tFROM cards INNER JOIN partners ON cards.PartnerID = partners.ID\n\t\t\t\t\tWHERE cards.ID={$tid}")->fetch_object()->Tel;
        $_GET['Address'] = $conn->query("SELECT partners.Address\n\t\t\t\t\tFROM cards INNER JOIN partners ON cards.PartnerID = partners.ID\n\t\t\t\t\tWHERE cards.ID={$tid}")->fetch_object()->Address;
        $_GET['WebSite'] = $conn->query("SELECT partners.WebSite\n\t\t\t\t\tFROM cards INNER JOIN partners ON cards.PartnerID = partners.ID\n\t\t\t\t\tWHERE cards.ID={$tid}")->fetch_object()->WebSite;
        $_GET['Email'] = $conn->query("SELECT partners.Email\n\t\t\t\t\tFROM cards INNER JOIN partners ON cards.PartnerID = partners.ID\n\t\t\t\t\tWHERE cards.ID={$tid}")->fetch_object()->Email;
        $_GET['Facebook'] = $conn->query("SELECT partners.Facebook\n\t\t\t\t\tFROM cards INNER JOIN partners ON cards.PartnerID = partners.ID\n\t\t\t\t\tWHERE cards.ID={$tid}")->fetch_object()->Facebook;
        $_GET['Instagram'] = $conn->query("SELECT partners.Instagram\n\t\t\t\t\tFROM cards INNER JOIN partners ON cards.PartnerID = partners.ID\n\t\t\t\t\tWHERE cards.ID={$tid}")->fetch_object()->Instagram;
        $_GET['Twitter'] = $conn->query("SELECT partners.Twitter\n\t\t\t\t\tFROM cards INNER JOIN partners ON cards.PartnerID = partners.ID\n\t\t\t\t\tWHERE cards.ID={$tid}")->fetch_object()->Twitter;
        $yourArray = array();
        // make a new array to hold all your data
        $index = 0;
        while ($row = mysqli_fetch_assoc($result)) {
            // loop to store the data in an associative array.
            $yourArray[$index] = $row;
            $index++;
        }
        $_GET['Photos'] = $yourArray;
        echo json_encode($_GET);
        include '../close.php';
    } else {
        echo "404 NOT FOUND";
    }
}
function carousel()
{
    include '../init.php';
    $sql = "SELECT CardID , Location FROM mainslide";
    $result = mysqli_query($conn, $sql);
    // This line executes the MySQL query that you typed above
    $slidearray = array();
    // make a new array to hold all your data
    $index = 0;
    while ($row = mysqli_fetch_assoc($result)) {
        // loop to store the data in an associative array.
        $slidearray[$index] = $row;
        $index++;
    }
    echo json_encode($slidearray);
    include '../close.php';
}
function questions()
{
    include '../init.php';
    $card_id = $_GET['id'];
    $sql = "SELECT  Question, ID FROM questions WHERE CardID={$card_id}";
    $result = mysqli_query($conn, $sql);
    // This line executes the MySQL query that you typed above
    $questionarray = array();
    // make a new array to hold all your data
    $answers = array();
    $index = 0;
    while ($row = mysqli_fetch_assoc($result)) {
        // loop to store the data in an associative array.
        $questionarray[$index][0] = $row;
        $question_id = $row['ID'];
        $sql_answer = "SELECT Answer, ID FROM answers WHERE QuestionID={$question_id}";
        $result_answer = mysqli_query($conn, $sql_answer);
        $index_answer = 0;
        while ($row_answer = mysqli_fetch_assoc($result_answer)) {
            $answers[$index_answer] = $row_answer;
            $index_answer++;
        }
        /*
        
               while ($row_answer1 = mysqli_fetch_array($answerarray))
               {
                   $questionarray[$index][1] = $row_answer1;
               }
        */
        $questionarray[$index][1] = $answers;
        $index++;
    }
    echo json_encode($questionarray);
    include '../close.php';
}
if ($_GET['view'] == 'topInfo') {
    topinfo();
} else {
    if ($_GET['view'] == 'newInfo') {
        newinfo();
    } else {
        if ($_GET['view'] == 'cardInfo') {
            cardinfo();
        } else {
            if ($_GET['view'] == 'carousel') {
                carousel();
            } else {
                if ($_GET['view'] == 'questions') {
                    questions();
                } else {
                    echo "nothing";
                }
            }
        }
    }
}
