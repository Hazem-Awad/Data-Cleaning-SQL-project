# Data-Cleaning-SQL-project
SELECT *
FROM PorfolioProject..NashvilleHousing -- Standardize Date Format

SELECT SaleDateConverted,
       CONVERT(date, SaleDate)
FROM PorfolioProject..NashvilleHousing
UPDATE NashvilleHousing
SET SaleDate = CONVERT(date, SaleDate)
ALTER TABLE NashvilleHousing ADD SaleDateConverted Date;


UPDATE NashvilleHousing
SET SaleDateConverted = CONVERT(date, SaleDate) -- Populate Property Address data

SELECT *
FROM PorfolioProject.dbo.NashvilleHousing --Where PropertyAddress is null
ORDER BY ParcelID
SELECT a.ParcelID,
       b.ParcelID,
       a.PropertyAddress,
       b.PropertyAddress,
       ISNULL(a.PropertyAddress, b.PropertyAddress)
FROM PorfolioProject.dbo.NashvilleHousing a
JOIN PorfolioProject.dbo.NashvilleHousing b ON a.ParcelID = b.ParcelID
AND a.[UniqueID ] <> b.[UniqueID ]
WHERE a.PropertyAddress IS NULL
  SELECT PropertyAddress
  FROM PorfolioProject.dbo.NashvilleHousing WHERE PropertyAddress IS NULL
  UPDATE a
  SET propertyAddress = ISNULL(a.PropertyAddress, b.PropertyAddress)
  FROM PorfolioProject.dbo.NashvilleHousing a
  JOIN PorfolioProject.dbo.NashvilleHousing b ON a.ParcelID = b.ParcelID
  AND a.[UniqueID ] <> b.[UniqueID ] WHERE a.PropertyAddress IS NULL -- Breaking out Address into Individual Columns (Address, City, State)

  SELECT PropertyAddress
  FROM PorfolioProject.dbo.NashvilleHousing --Where PropertyAddress is null
--order by ParcelID

  SELECT SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) -1) AS Address,
         SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1, LEN(PropertyAddress)) AS Address
  FROM PorfolioProject.dbo.NashvilleHousing
  ALTER TABLE NashvilleHousing ADD PropertySplityAddress Nvarchar(225);


UPDATE NashvilleHousing
SET PropertySplityAddress = SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) -1)
ALTER TABLE NashvilleHousing ADD PropertySplitCity Nvarchar(225);


UPDATE NashvilleHousing
SET PropertySplitCity = SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1, LEN(PropertyAddress))
SELECT *
FROM PorfolioProject.dbo.NashvilleHousing
SELECT OwnerAddress
FROM PorfolioProject.dbo.NashvilleHousing
SELECT PARSENAME(REPLACE(OwnerAddress, ',', '.'), 3),
       PARSENAME(REPLACE(OwnerAddress, ',', '.'), 2),
       PARSENAME(REPLACE(OwnerAddress, ',', '.'), 1)
FROM PorfolioProject.dbo.NashvilleHousing
ALTER TABLE NashvilleHousing ADD OwnerSplitAddress Nvarchar(225);


UPDATE NashvilleHousing
SET OwnerSplitAddress = PARSENAME(REPLACE(OwnerAddress, ',', '.'), 3)
ALTER TABLE NashvilleHousing ADD OwnerSplitCity Nvarchar(225);


UPDATE NashvilleHousing
SET OwnerSplitCity = PARSENAME(REPLACE(OwnerAddress, ',', '.'), 2)
ALTER TABLE NashvilleHousing ADD OwnerSplitState Nvarchar(255);


UPDATE NashvilleHousing
SET OwnerSplitState = PARSENAME(REPLACE(OwnerAddress, ',', '.'), 1)
SELECT *
FROM PorfolioProject.dbo.NashvilleHousing -- Change Y and N to Yes and No in "Sold as Vacant" field

SELECT Distinct(SoldAsVacant),
       Count(SoldAsVacant)
FROM PorfolioProject.dbo.NashvilleHousing
GROUP BY SoldAsVacant
ORDER BY 2
SELECT SoldAsVacant,
       CASE
           WHEN SoldAsVacant = 'Y' THEN 'Yes'
           WHEN SoldAsVacant = 'N' THEN 'No'
           ELSE SoldAsVacant
       END
FROM PorfolioProject.dbo.NashvilleHousing
UPDATE NashvilleHousing
SET SoldAsVacant = CASE
                       WHEN SoldAsVacant = 'Y' THEN 'Yes'
                       WHEN SoldAsVacant = 'N' THEN 'No'
                       ELSE SoldAsVacant
                   END -----------------------------------------------------------------------------------------------------------------------------------------------------------
 -- Remove Duplicates
 WITH RowNumCTE AS
  (SELECT *,
          ROW_NUMBER() OVER (PARTITION BY ParcelID,
                                          PropertyAddress,
                                          SalePrice,
                                          SaleDate,
                                          LegalReference
                             ORDER BY UniqueID) row_num
   FROM PorfolioProject.dbo.NashvilleHousing --order by ParcelID
)
SELECT *
FROM RowNumCTE
WHERE row_num > 1
ORDER BY PropertyAddress -- Delete Unused Columns

SELECT *
FROM PorfolioProject.dbo.NashvilleHousing
ALTER TABLE PorfolioProject.dbo.NashvilleHousing
DROP COLUMN OwnerAddress,
            TaxDistrict,
            PropertyAddress,
            SaleDate
