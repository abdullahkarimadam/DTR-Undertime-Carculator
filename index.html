<?php
session_start();

// Simple data storage (pwede mo palitan ng SQLite or MySQL later)
$dataFile = 'dtr_data.json';
if (!file_exists($dataFile)) {
    file_put_contents($dataFile, json_encode(['users' => [], 'custom_holidays' => []]));
}

$data = json_decode(file_get_contents($dataFile), true);
$users = $data['users'] ?? [];
$customHolidays = $data['custom_holidays'] ?? [];

// Philippine Regular Holidays 2025-2026 (auto-update yearly)
function getPHRegularHolidays($year) {
    $holidays = [
        "$year-01-01" => "New Year's Day",
        "$year-04-17" => "Maundy Thursday",
        "$year-04-18" => "Good Friday",
        "$year-04-09" => "Araw ng Kagitingan",
        "$year-05-01" => "Labor Day",
        "$year-06-12" => "Independence Day",
        "$year-08-21" => "Ninoy Aquino Day",
        "$year-08-26" => "National Heroes Day (Last Monday of August)",
        "$year-11-30" => "Bonifacio Day",
        "$year-12-25" => "Christmas Day",
        "$year-12-30" => "Rizal Day",
        "$year-12-31" => "Last Day of the Year",
    ];
    // Adjust movable holidays
    $easter = easter_date($year);
    $holidays[date('Y-m-d', $easter - 3*86400)] = "Maundy Thursday";
    $holidays[date('Y-m-d', $easter - 2*86400)] = "Good Friday";
    return $holidays;
}

$year = date('Y');
$phHolidays = getPHRegularHolidays($year);
$phHolidaysNext = getPHRegularHolidays($year + 1);
$allHolidays = array_merge($phHolidays, $phHolidaysNext, array_fill_keys($customHolidays, "Custom Holiday"));

// Current user
$currentUser = $_SESSION['user'] ?? 'default';
if (!isset($users[$currentUser])) {
    $users[$currentUser] = [
        'name' => $currentUser === 'default' ? 'Ako' : ucfirst($currentUser),
        'schedule' => [
            'am_in' => '08:00',
            'am_out' => '12:00',
            'pm_in' => '13:00',
            'pm_out' => '17:00',
        ],
        'records' => []
    ];
    file_put_contents($dataFile, json_encode(['users' => $users, 'custom_holidays' => $customHolidays]));
}

// Handle form submissions
if ($_POST) {
    if (isset($_POST['add_user'])) {
        $newUser = strtolower(trim($_POST['new_user']));
        if ($newUser && !isset($users[$newUser])) {
            $users[$newUser] = [
                'name' => ucwords($newUser),
                'schedule' => $users[$currentUser]['schedule'],
                'records' => []
            ];
        }
    }

    if (isset($_POST['switch_user'])) {
        $_SESSION['user'] = $_POST['user_select'];
        header('Location: ' . $_SERVER['PHP_SELF']);
        exit;
    }

    if (isset($_POST['save_schedule'])) {
        $users[$currentUser]['schedule'] = [
            'am_in' => $_POST['am_in'],
            'am_out' => $_POST['am_out'],
            'pm_in' => $_POST['pm_in'],
            'pm_out' => $_POST['pm_out'],
        ];
    }

    if (isset($_POST['add_record'])) {
        $date = $_POST['date'];
        $users[$currentUser]['records'][$date] = [
            'am_in' => $_POST['rec_am_in'],
            'am_out' => $_POST['rec_am_out'],
            'pm_in' => $_POST['rec_pm_in'],
            'pm_out' => $_POST['rec_pm_out'],
        ];
    }

    if (isset($_POST['add_holiday'])) {
        $hdate = $_POST['holiday_date'];
        if (!in_array($hdate, $customHolidays)) {
            $customHolidays[] = $hdate;
        }
    }

    if (isset($_POST['delete_record'])) {
        unset($users[$currentUser]['records'][$_POST['delete_record']]);
    }

    file_put_contents($dataFile, json_encode(['users' => $users, 'custom_holidays' => $customHolidays]));
    header('Location: ' . $_SERVER['PHP_SELF']);
    exit;
}

$user = $users[$currentUser];
$schedule = $user['schedule'];
$records = $user['records'];

// Calculate undertime
function minutesDiff($time1, $time2) {
    return (strtotime($time2) - strtotime($time1)) / 60;
}

function calculateUndertime($record, $schedule, $isHoliday = false) {
    if ($isHoliday || empty($record['am_in'])) return 0;

    $required = 0;
    $rendered = 0;

    // AM
    if ($record['am_in'] && $record['am_out']) {
        $late = max(0, minutesDiff($schedule['am_in'], $record['am_in']));
        $early = $record['am_out'] < $schedule['am_out'] ? minutesDiff($record['am_out'], $schedule['am_out']) : 0;
        $rendered += minutesDiff($record['am_in'], $record['am_out']);
        $required += minutesDiff($schedule['am_in'], $schedule['am_out']);
    }

    // PM
    if ($record['pm_in'] && $record['pm_out']) {
        $late = max(0, minutesDiff($schedule['pm_in'], $record['pm_in']));
        $early = $record['pm_out'] < $schedule['pm_out'] ? minutesDiff($record['pm_out'], $schedule['pm_out']) : 0;
        $rendered += minutesDiff($record['pm_in'], $record['pm_out']);
        $required += minutesDiff($schedule['pm_in'], $schedule['pm_out']);
    }

    $undertime = max(0, $required - $rendered);
    return round($undertime);
}

$currentMonth = date('Y-m');
?>

<!DOCTYPE html>
<html lang="tl" class="h-full bg-gray-50">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DTR Undertime Calculator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
</head>
<body class="h-full">
<div class="min-h-full flex flex-col">
    <!-- Header -->
    <header class="bg-indigo-600 text-white p-4 shadow-lg">
        <div class="max-w-6xl mx-auto flex justify-between items-center">
            <h1 class="text-2xl font-bold"><i class="fas fa-clock mr-2"></i> DTR Undertime Calculator</h1>
            <form method="POST" class="flex items-center gap-2">
                <select name="user_select" onchange="this.form.submit()" class="bg-white text-gray-800 rounded px-3 py-2 text-sm">
                    <?php foreach ($users as $key => $u): ?>
                        <option value="<?= $key ?>" <?= $currentUser === $key ? 'selected' : '' ?>><?= $u['name'] ?></option>
                    <?php endforeach; ?>
                </select>
                <button type="submit" name="switch_user" class="hidden">Switch</button>
            </form>
        </div>
    </header>

    <div class="flex-grow max-w-6xl mx-auto w-full p-4">
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">

            <!-- Schedule Setup -->
            <div class="bg-white rounded-xl shadow-md p-6">
                <h2 class="text-xl font-bold text-indigo-600 mb-4"><i class="fas fa-cog"></i> Schedule Setup</h2>
                <form method="POST" class="space-y-3">
                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-sm font-medium">AM Arrival</label>
                            <input type="time" name="am_in" value="<?= $schedule['am_in'] ?>" class="w-full border rounded-lg px-3 py-2">
                        </div>
                        <div>
                            <label class="block text-sm font-medium">AM Departure</label>
                            <input type="time" name="am_out" value="<?= $schedule['am_out'] ?>" class="w-full border rounded-lg px-3 py-2">
                        </div>
                        <div>
                            <label class="block text-sm font-medium">PM Arrival</label>
                            <input type="time" name="pm_in" value="<?= $schedule['pm_in'] ?>" class="w-full border rounded-lg px-3 py-2">
                        </div>
                        <div>
                            <label class="block text-sm font-medium">PM Departure</label>
                            <input type="time" name="pm_out" value="<?= $schedule['pm_out'] ?>" class="w-full border rounded-lg px-3 py-2">
                        </div>
                    </div>
                    <button name="save_schedule" class="w-full bg-indigo-600 text-white py-2 rounded-lg hover:bg-indigo-700 transition">
                        <i class="fas fa-save"></i> Save Schedule
                    </button>
                </form>

                <hr class="my-6">

                <h3 class="font-bold text-gray-700 mb-3">Add User</h3>
                <form method="POST" class="flex gap-2">
                    <input type="text" name="new_user" placeholder="Pangalan" class="flex-1 border rounded-lg px-3 py-2" required>
                    <button name="add_user" class="bg-green-600 text-white px-4 rounded-lg hover:bg-green-700"><i class="fas fa-plus"></i></button>
                </form>

                <hr class="my-6">

                <h3 class="font-bold text-gray-700 mb-3">Add Custom Holiday</h3>
                <form method="POST" class="flex gap-2">
                    <input type="date" name="holiday_date" class="flex-1 border rounded-lg px-3 py-2" required>
                    <button name="add_holiday" class="bg-orange-600 text-white px-4 rounded-lg hover:bg-orange-700"><i class="fas fa-plus"></i></button>
                </form>
            </div>

            <!-- Monthly View -->
            <div class="lg:col-span-2 bg-white rounded-xl shadow-md p-6 overflow-x-auto">
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-2xl font-bold text-indigo-600">
                        <?= date('F Y') ?>
                    </h2>
                    <div class="text-sm text-gray-500">
                        <i class="fas fa-circle text-red-500"></i> Holiday &nbsp; 
                        <i class="fas fa-circle text-yellow-500"></i> May Undertime
                    </div>
                </div>

                <div class="grid grid-cols-7 gap-2 text-center font-semibold text-sm">
                    <?php foreach(['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'] as $day): ?>
                        <div class="py-3 text-gray-600"><?= $day ?></div>
                    <?php endforeach; ?>
                </div>

                <div class="grid grid-cols-7 gap-2 text-sm">
                    <?php
                    $firstDay = date('w', strtotime("$currentMonth-01"));
                    $daysInMonth = date('t');
                    for ($i = 0; $i < $firstDay; $i++) echo "<div></div>";

                    for ($day = 1; $day <= $daysInMonth; $day++) {
                        $dateStr = sprintf("%s-%02d", $currentMonth, $day);
                        $isHoliday = isset($allHolidays[$dateStr]);
                        $record = $records[$dateStr] ?? null;
                        $undertime = $record ? calculateUndertime($record, $schedule, $isHoliday) : 0;
                        $hasUT = $undertime > 0;

                        echo "<div class='border rounded-lg p-2 min-h-24 flex flex-col justify-between " .
                            ($isHoliday ? 'bg-red-50 border-red-300' : '') .
                            ($hasUT ? 'bg-yellow-50 border-yellow-400' : '') .
                            "'>";
                        echo "<div class='font-bold text-right text-xs'>$day</div>";
                        if ($record) {
                            echo "<div class='text-xs'>";
                            if ($record['am_in']) echo "AM: {$record['am_in']}-{$record['am_out']}<br>";
                            if ($record['pm_in']) echo "PM: {$record['pm_in']}-{$record['pm_out']}";
                            echo "</div>";
                            if ($undertime > 0) echo "<div class='text-red-600 font-bold text-xs'>UT: {$undertime}min</div>";
                        }
                        echo "</div>";
                    }
                    ?>
                </div>

                <!-- Add Record Modal Trigger -->
                <button onclick="document.getElementById('addModal').classList.remove('hidden')" 
                    class="mt-6 fixed bottom-6 right-6 bg-indigo-600 text-white w-14 h-14 rounded-full shadow-lg hover:bg-indigo-700 text-2xl">
                    <i class="fas fa-plus"></i>
                </button>
            </div>
        </div>
    </div>

    <!-- Add Record Modal -->
    <div id="addModal" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
        <div class="bg-white rounded-xl p-6 w-full max-w-md mx-4">
            <h3 class="text-xl font-bold mb-4">Log Time - <?= $user['name'] ?></h3>
            <form method="POST">
                <div class="mb-4">
                    <label class="block font-medium">Date</label>
                    <input type="date" name="date" value="<?= date('Y-m-d') ?>" class="w-full border rounded-lg px-3 py-2" required>
                </div>
                <div class="grid grid-cols-2 gap-3">
                    <input type="time" name="rec_am_in" placeholder="AM In" class="border rounded-lg px-3 py-2">
                    <input type="time" name="rec_am_out" placeholder="AM Out" class="border rounded-lg px-3 py-2">
                    <input type="time" name="rec_pm_in" placeholder="PM In" class="border rounded-lg px-3 py-2">
                    <input type="time" name="rec_pm_out" placeholder="PM Out" class="border rounded-lg px-3 py-2">
                </div>
                <div class="flex gap-3 mt-6">
                    <button name="add_record" class="flex-1 bg-indigo-600 text-white py-2 rounded-lg hover:bg-indigo-700">
                        Save Record
                    </button>
                    <button type="button" onclick="document.getElementById('addModal').classList.add('hidden')" 
                        class="flex-1 bg-gray-500 text-white py-2 rounded-lg hover:bg-gray-600">
                        Cancel
                    </button>
                </div>
            </form>
        </div>
    </div>
</div>
</body>
</html>