// ==UserScript==
// @name         Extract Attendee Info to CSV
// @namespace    http://tampermonkey.net/
// @version      2025-07-21
// @description  Extracts WordCamp US 2025 attendee list to a csv file
// @author       joweber123
// @match        https://us.wordcamp.org/2025/attendees/
// @icon         https://www.google.com/s2/favicons?sz=64&domain=wordcamp.org
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    function extractAndDownloadCSV() {
        const attendees = document.querySelectorAll('li'); // Select all <li> elements, adjust if there's a more specific parent for attendees
        let csvContent = "First Name,Last Name,Badge First Name,Badge Last Name,Job Title,WordPress.org Profile,Company/Personal Website\n";

        attendees.forEach(attendee => {
            // Check if the current <li> element contains attendee information
            if (attendee.querySelector('.tix-attendee-name')) {
                const firstName = attendee.querySelector('.tix-first')?.textContent.trim() || '';
                const lastName = attendee.querySelector('.tix-last')?.textContent.trim() || '';
                const badgeFirstName = attendee.querySelector('.tix-badge-first-name')?.textContent.trim() || '';
                const badgeLastName = attendee.querySelector('.tix-badge-last-name')?.textContent.trim() || '';
                const jobTitle = attendee.querySelector('.tix-job-title')?.textContent.trim() || '';
                const wordpressProfile = attendee.querySelector('.tix-wordpress-org-profile-url')?.href || '';
                const companyWebsite = attendee.querySelector('.tix-company-or-personal-website')?.href || '';

                // Escape commas and newlines within the data to prevent CSV corruption
                const escapeCSV = (field) => {
                    if (field.includes(',') || field.includes('\n') || field.includes('"')) {
                        return `"${field.replace(/"/g, '""')}"`;
                    }
                    return field;
                };

                csvContent += `${escapeCSV(firstName)},${escapeCSV(lastName)},${escapeCSV(badgeFirstName)},${escapeCSV(badgeLastName)},${escapeCSV(jobTitle)},${escapeCSV(wordpressProfile)},${escapeCSV(companyWebsite)}\n`;
            }
        });

        if (attendees.length > 0) {
            // Create a Blob and initiate download
            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement('a');
            if (link.download !== undefined) { // Feature detection for download attribute
                const url = URL.createObjectURL(blob);
                link.setAttribute('href', url);
                link.setAttribute('download', 'attendee_info.csv');
                link.style.visibility = 'hidden';
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
                alert('Attendee information exported to attendee_info.csv!');
            } else {
                // Fallback for browsers that do not support the download attribute
                alert('Your browser does not support automatic downloads. Please copy the following CSV content manually:\n\n' + csvContent);
            }
        } else {
            alert('No attendee information found on this page.');
        }
    }

    // Add a button to trigger the extraction
    function addExtractButton() {
        const button = document.createElement('button');
        button.textContent = 'Extract Attendee Data to CSV';
        button.style.cssText = `
            position: fixed;
            top: 10px;
            right: 10px;
            z-index: 10000;
            padding: 10px 15px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
        `;
        button.onclick = extractAndDownloadCSV;
        document.body.appendChild(button);
    }

    // Run the function when the page loads
    window.addEventListener('load', addExtractButton);

})();
